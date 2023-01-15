# Self-managed node group to managed node group 마이그레이션 - eksctl 이용

<br>

## 시나리오
Self-managed nodes에서 구동 중인 pods를 Managed nodes로 마이그레이션

<br>

## 테스트 환경 구축
### EKS cluster 생성
```bash
eksctl create cluster \
  --name my-cluster \
  --region ap-northeast-2 \
  --version 1.23 \
  --vpc-public-subnets subnet-053d96d87e10981ff,subnet-0219235848224a0b1,subnet-067ce4c7b310680ba \
  --without-nodegroup
```

### EKS self-managed node group 생성
```bash
eksctl create nodegroup \
  --cluster my-cluster \
  --name al-nodes \
  --node-type t3.small \
  --nodes 3 \
  --nodes-min 1 \
  --nodes-max 4 \
  --ssh-access \
  --managed=false \
  --ssh-public-key bigmtn
```

### Sample application 생성
```bash
kubectl apply -f https://k8s.io/examples/controllers/nginx-deployment.yaml
```

### 확인
nodes, deployments, replicasets, pods 확인

```bash
kubectl get nodes,deploy,rs,pods -A
```

<br>

## 절차
### 1. 관리형 노드 그룹 생성
```bash
eksctl create nodegroup \
  --cluster my-cluster \
  --region ap-northeast-2 \
  --name my-mng \
  --node-ami-family AmazonLinux2 \
  --node-type t3.small \
  --nodes 3 \
  --nodes-min 2 \
  --nodes-max 4 \
  --ssh-access \
  --ssh-public-key bigmtn
```

### 2. Self-managed node group의 모든 node를 "NoSchedule"로 오염
EKS scheduler가 Self-managed nodes에 새 pods를 스케줄링하지 않도록 지시

Self-managed nodes는 node group의 이름을 label로 갖고 있으므로 모든 self-managed nodes에 taint를 적용하려면 다음 명령을 사용  
```bash
kubectl taint node -l "alpha.eksctl.io/nodegroup-name"="<<SELF-MANAGED-NODE-GROUP-NAME>>" key=value:NoSchedule
```

### 3. Application deployments scale-out
Application pods가 managed nodes에 스케줄링되도록 application replicas 값 증대

Self-managed nodes는 taint되었으므로 replicas 수가 증가하면 새 pods는 managed nodes에 배포  
프로덕션 애플리케이션의 경우 replicas 수를 100% 늘리는 것을 권장

```bash
kubectl scale deployments/<<DEPLOYMENT-NAME>> --replicas=4
```

### 4. kube-system의 deployments scale-out
이 설정에서는 두 가지 deployments를 안내
- core-dns
- aws=load-balancer-controller

Application pods 수를 늘리는 것처럼 동일한 방식으로 진행  
초기 설정에는 두 개의 복제본 존재

```bash
kubectl scale deployments/coredns --replicas=4 -n kube-system
kubectl scale deployments/aws-load-balancer-controller --replicas=4 -n kube-system
```

### 5. 새 pods들 확인
Application pods와 kube-system 관련 pods들이 정상적으로 실행 중인지 확인

```bash
kubectl get replicasets -A
```

### 6. Self-managed nodes를 비우고 self-managed node group 삭제


```bash
kubectl drain =l "alpha.eksctl.io/nodegroup-name"="<<SELF-MANAGED-NODE-GROUP-NAME>>" --ignore-daemonsets --delete-emptydir-data
```

### 7. deployments scale-in
필요에 맞게 replicas 값 축소

```bash
kubectl scale deployments/<<DEPLOYMENT-NAME>> --replicas=2
kubectl scale deployments/coredns --replicas=2 -n kube-system
kubectl scale deployments/aws-load-balancer-controller --replicas=2 -n kube-system
```

<hr>

## 참고
- **Self-managed node group to managed node group 마이그레이션** - https://aws.amazon.com/ko/blogs/containers/seamlessly-migrate-workloads-from-eks-self-managed-node-group-to-eks-managed-node-groups/
