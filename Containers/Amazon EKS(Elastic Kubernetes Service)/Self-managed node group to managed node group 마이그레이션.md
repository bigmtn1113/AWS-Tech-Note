# Self-managed node group to managed node group 마이그레이션 - eksctl 이용

<br>

## 시나리오
Self-managed nodes에서 구동 중인 pods를 Managed nodes로 마이그레이션

<br>

## 테스트 환경 구축
### EKS cluster 생성
```bash
$ eksctl create cluster \
  --name my-cluster \
  --region ap-northeast-2 \
  --version 1.23 \
  --vpc-public-subnets subnet-053d96d87e10981ff,subnet-0219235848224a0b1,subnet-067ce4c7b310680ba \
  --without-nodegroup
```

### EKS self-managed node group 생성
```bash
$ eksctl create nodegroup \
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
$ kubectl apply -f https://k8s.io/examples/controllers/nginx-deployment.yaml
```

### 리소스 확인
nodes, daemonsets, deployments, replicasets, pods 확인

```bash
$ kubectl get nodes,ds,deploy,rs,pods -A
```

<br>

## 절차
### 1. 관리형 노드 그룹 생성
```bash
$ eksctl create nodegroup \
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

#### 리소스 확인
```bash
$ kubectl get nodes,ds,deploy,rs,pods -A
```

### 2. Self-managed node group의 모든 node를 "NoSchedule"로 오염
EKS scheduler가 Self-managed nodes에 새 pods를 스케줄링하지 않도록 지시

Self-managed nodes는 node group의 이름을 label로 갖고 있으므로 모든 self-managed nodes에 taint를 적용하려면 다음 명령을 사용  
```bash
# kubectl taint node -l "alpha.eksctl.io/nodegroup-name"="<<SELF-MANAGED-NODE-GROUP-NAME>>" key=value:NoSchedule
$ kubectl taint node -l "alpha.eksctl.io/nodegroup-name"="al-nodes" key=value:NoSchedule

node/ip-172-31-0-51.ap-northeast-2.compute.internal tainted
node/ip-172-31-30-138.ap-northeast-2.compute.internal tainted
node/ip-172-31-35-91.ap-northeast-2.compute.internal tainted
```

#### Tainted nodes 확인
tainted 되었는지 `kubectl describe` 명령어로 확인

```bash
$ kubectl describe node/ip-172-31-0-51.ap-northeast-2.compute.internal | grep Taints
$ kubectl describe node/ip-172-31-30-138.ap-northeast-2.compute.internal | grep Taints
$ kubectl describe node/ip-172-31-35-91.ap-northeast-2.compute.internal | grep Taints

Taints:             key=value:NoSchedule
```

### 3. Application deployments scale-out
Application pods가 managed nodes에 스케줄링되도록 application replicas 값 증대

Self-managed nodes는 taint되었으므로 replicas 수가 증가하면 새 pods는 managed nodes에 배포  
프로덕션 애플리케이션의 경우 replicas 수를 100% 늘리는 것을 권장

```bash
# kubectl scale deployments/<<DEPLOYMENT-NAME>> --replicas=6
$ kubectl scale deployments/nginx-deployment --replicas=6
```

#### Pods 확인
새 pods가 managed nodes에 배포된 것 확인 가능

```bash
$ kubectl get pods -o wide
```

### 4. kube-system의 deployments scale-out
현재 kube-system에 code-dns만 있으므로 code-dns만 진행

Application pods 수를 늘리는 것처럼 동일한 방식으로 진행  
초기 설정에는 두 개의 복제본만 존재하므로 replicas 값은 4

```bash
$ kubectl scale deployments/coredns --replicas=4 -n kube-system
```

### 5. 새 pods들 확인
Application pods와 kube-system 관련 pods들이 정상적으로 실행 중인지 확인

```bash
$ kubectl get replicasets -A
```

### 6. Self-managed nodes 비우기
```bash
# kubectl drain -l "alpha.eksctl.io/nodegroup-name"="<<SELF-MANAGED-NODE-GROUP-NAME>>" --ignore-daemonsets --delete-emptydir-data
$ kubectl drain -l "alpha.eksctl.io/nodegroup-name"="al-nodes" --ignore-daemonsets --delete-emptydir-data

node/ip-172-31-0-51.ap-northeast-2.compute.internal cordoned
node/ip-172-31-30-138.ap-northeast-2.compute.internal cordoned
node/ip-172-31-35-91.ap-northeast-2.compute.internal cordoned
WARNING: ignoring DaemonSet-managed Pods: kube-system/aws-node-txlkg, kube-system/kube-proxy-q9mdk
evicting pod default/nginx-deployment-9456bbbf9-mxtsh
evicting pod kube-system/coredns-d596d9655-2xb27
evicting pod kube-system/coredns-d596d9655-6nd2w
pod/coredns-d596d9655-2xb27 evicted
pod/nginx-deployment-9456bbbf9-mxtsh evicted
pod/coredns-d596d9655-6nd2w evicted
node/ip-172-31-0-51.ap-northeast-2.compute.internal evicted
WARNING: ignoring DaemonSet-managed Pods: kube-system/aws-node-th7wf, kube-system/kube-proxy-qm9gm
evicting pod default/nginx-deployment-9456bbbf9-x4ftz
pod/nginx-deployment-9456bbbf9-x4ftz evicted
node/ip-172-31-30-138.ap-northeast-2.compute.internal evicted
WARNING: ignoring DaemonSet-managed Pods: kube-system/aws-node-tmp5l, kube-system/kube-proxy-gbg6p
evicting pod default/nginx-deployment-9456bbbf9-5n2ft
pod/nginx-deployment-9456bbbf9-5n2ft evicted
node/ip-172-31-35-91.ap-northeast-2.compute.internal evicted
```

※ `--ignore-daemonsets`  
Daemonsets으로 실행된 pods는 삭제해도 daemonsets이 즉시 다시 실행하므로 무시하는 옵션을 주고 진행

#### Nodes 확인
Self-managed nodes의 STATUS에 `SchedulingDisabled` 표시가 생성된 것 확인 가능

```bash
$ kubectl get nodes

NAME                                               STATUS                     ROLES    AGE   VERSION
ip-172-31-0-51.ap-northeast-2.compute.internal     Ready,SchedulingDisabled   <none>   73m   v1.23.13-eks-fb459a0
ip-172-31-20-1.ap-northeast-2.compute.internal     Ready                      <none>   43m   v1.23.13-eks-fb459a0
ip-172-31-30-138.ap-northeast-2.compute.internal   Ready,SchedulingDisabled   <none>   73m   v1.23.13-eks-fb459a0
ip-172-31-35-91.ap-northeast-2.compute.internal    Ready,SchedulingDisabled   <none>   73m   v1.23.13-eks-fb459a0
ip-172-31-4-153.ap-northeast-2.compute.internal    Ready                      <none>   43m   v1.23.13-eks-fb459a0
ip-172-31-47-67.ap-northeast-2.compute.internal    Ready                      <none>   43m   v1.23.13-eks-fb459a0
```

#### Pods 확인
`kubectl drain` 명령어로 인해 self-managed nodes에 있던 nginx-deployment와 coredns의 pods가 제거되고 managed nodes에서 새롭게 생성된 것 확인 가능

```bash
$ kubectl get pods -A -o wide
```

### 7. Self-managed node group 제거
```bash
$ eksctl delete nodegroup \
  --cluster my-cluster \
  --region ap-northeast-2 \
  --name al-nodes
```

### 8. deployments scale-in
필요에 맞게 replicas 값 축소

```bash
# kubectl scale deployments/<<DEPLOYMENT-NAME>> --replicas=3
$ kubectl scale deployments/nginx-deployment --replicas=3
$ kubectl scale deployments/coredns --replicas=2 -n kube-system
```

#### 리소스 확인
nodes, daemonsets, deployments, replicasets, pods 확인

```bash
$ kubectl get nodes,ds,deploy,rs,pods -A
```

<hr>

## 참고
- **Self-managed node group to managed node group 마이그레이션** - https://aws.amazon.com/ko/blogs/containers/seamlessly-migrate-workloads-from-eks-self-managed-node-group-to-eks-managed-node-groups/
- **Amazon EKS cluster 생성** - https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html
- **Self-managed Amazon Linux nodes 생성** - https://docs.aws.amazon.com/eks/latest/userguide/launch-workers.html
- **Deployments 생성** - https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#creating-a-deployment
- **Managed node group 생성** - https://docs.aws.amazon.com/eks/latest/userguide/launch-workers.html
