# VPC Peering Connection

<br/>

## Architecture 참고
![Image](https://user-images.githubusercontent.com/46125158/173216996-20938f3a-69b9-4ae3-98df-74a2c0a9a496.png)

<hr>

## 사전 작업
### VPC Peering Connection할 두 VPC 생성
![Cap 2022-06-11 18-18-13-168](https://user-images.githubusercontent.com/46125158/173217138-3512a457-c842-4e29-96fa-092ced644c1e.png)

<hr>

## VPC Peering Connection 설정
### VPC Peering Connection 생성
![Cap 2022-06-11 18-20-09-699](https://user-images.githubusercontent.com/46125158/173217159-ce88d606-740c-4184-9150-15d195ca2216.png)  
![Cap 2022-06-11 18-21-39-178](https://user-images.githubusercontent.com/46125158/173217323-2edc2667-c31b-4bac-bcad-f5f196c202bb.png)  
![Cap 2022-06-11 18-22-01-045](https://user-images.githubusercontent.com/46125158/173217325-ef473bb4-24fd-4552-81b4-665215525af0.png)

### VPC Peering Connection 요청 확인 및 수락
![Cap 2022-06-11 18-23-11-019](https://user-images.githubusercontent.com/46125158/173217375-dc4eeac2-a82a-42a3-8402-7b743ee10449.png)  
![Cap 2022-06-11 18-24-04-202](https://user-images.githubusercontent.com/46125158/173217416-7bf9e704-5f2f-4998-9956-e29c564960df.png)

### VPC Peering Connection 확인
![Cap 2022-06-12 14-44-57-121](https://user-images.githubusercontent.com/46125158/173217519-05b52170-0642-443d-b676-e54fa560edd7.png)

<hr>

## Route Tables 설정
### 통신할 리소스가 있는 서브넷과 연결된 Requester VPC Route table 확인
![Cap 2022-06-11 18-41-53-476](https://user-images.githubusercontent.com/46125158/173217598-443557dc-cac9-454b-9412-27233bd2b8d4.png)

### 라우팅 추가
![Cap 2022-06-11 18-43-01-993](https://user-images.githubusercontent.com/46125158/173218202-1f6b0691-cb9f-4f44-b756-5b1e132250b4.png)

### 통신할 리소스가 있는 서브넷과 연결된 Accepter VPC Route table 확인
![Cap 2022-06-11 18-44-01-352](https://user-images.githubusercontent.com/46125158/173218697-4d44570c-c96f-4940-9645-b4e7f3121cea.png)

### 라우팅 추가
![Cap 2022-06-11 18-44-50-717](https://user-images.githubusercontent.com/46125158/173219059-683b13b0-8a7e-4312-b9da-cb713b8d0a8d.png)

<hr>

## VPC Peering Connection Test
### Ping Test를 위한 EC2 서버들 보안 그룹 인바운드 규칙 추가
![Cap 2022-06-12 15-05-03-523](https://user-images.githubusercontent.com/46125158/173219426-f4ebdb54-629f-4ed6-add5-fa780fe8380a.png)

### EC2 간의 Ping Test
![Cap 2022-06-11 21-49-39-224](https://user-images.githubusercontent.com/46125158/173219554-d762fbb2-6eeb-49b9-9db3-5bf9b03dba63.png)  
![Cap 2022-06-11 21-49-48-458](https://user-images.githubusercontent.com/46125158/173219557-afd3864d-5a15-45cc-bb1d-9f8c56a211e1.png)

**※ IP가 아닌 Private IP DNS name로 Ping Test를 하고 싶을 경우**  
![Cap 2022-06-12 15-39-00-482](https://user-images.githubusercontent.com/46125158/173220766-67e315f4-1d03-4bc7-bb71-d0c01afa2ffc.png)

**두 속성이 모두 활성화 되어 있어야 Amazon Route 53 Resolver 서버가 Amazon에서 제공한 프라이빗 DNS 호스트 이름 확인 가능**  
**Amazon Route 53의 Private Hosting Zone에서 정의된 사용자 지정 DNS 도메인 이름을 사용할 때도 필요**  
![Cap 2022-06-12 15-37-47-163](https://user-images.githubusercontent.com/46125158/173220791-c36090ab-6ced-4f42-899a-a79e424ebb82.png)  
![Cap 2022-06-12 15-37-58-270](https://user-images.githubusercontent.com/46125158/173220793-16809487-4dc9-4f5c-9672-1222caa47344.png)

<hr>

## 참고
- **VPC peering connection lifecycle** - https://docs.aws.amazon.com/vpc/latest/peering/vpc-peering-basics.html#vpc-peering-lifecycle
- **VPC DNS 속성** - https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html#vpc-dns-support
