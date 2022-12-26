# AWS Transit Gateway

<br>

## 시나리오
**AWS Trasit Gateway**를 이용해 VPC-A(10.10.0.0/16)에 있는 EC2-A 인스턴스에서 VPC-B(10.20.0.0/16)의 private subnet에 있는 EC2-B 인스턴스와 VPC-C(10.30.0.0/16)의 private subnet에 있는 EC2-C 인스턴스로 접근

<br>

## Architect
![image](https://user-images.githubusercontent.com/46125158/209547952-1ad7e817-39b7-4665-b433-cb23d4768422.png)

<br>

## Transit Gateway
### Transit Gateway Attachment
연결 대상이 VPC일 경우, AWS Transit Gateway는 VPC 서브넷 내에 elastic network interface를 배포  
이 ENI는 Transit Gateway에서 선택된 서브넷 간에 트래픽을 라우팅하는데 사용

### 가용 영역
Transit Gateway에 VPC를 연결할 때는 Transit Gateway가 하나 이상의 가용 영역을 사용하여 트래픽을 VPC 서브넷의 리소스로 라우팅  
Transit Gateway는 서브넷의 IP 주소 하나를 사용하여 해당 서브넷에 network interface를 배치

가용 영역을 활성화하면 지정된 서브넷이 아닌 해당 영역의 모든 서브넷으로 트래픽 라우팅 가능  
Transit Gateway Attachment가 없는 가용 영역에 상주하는 리소스는 Transit Gateway에 도달 불가

### 라우팅
Transit Gateway는 Transit Gateway 라우팅 테이블을 사용하여 연결 간에 IPv4 및 IPv6 패킷을 라우팅

<br>

## 세부 설정
### Transit Gateway Attachment 설정
- **VPC-A Attachment**
  VPC | Subnet
  --- | ---
  VPC-A(10.10.0.0/16) | ap-northeast-2a(Public subnet)
- **VPC-B Attachment**
  VPC | Subnet
  --- | ---
  VPC-B(10.20.0.0/16) | ap-northeast-2a(Private subnet)
- **VPC-C Attachment**
  VPC | Subnet
  --- | ---
  VPC-C(10.30.0.0/16) | ap-northeast-2a(Private subnet)
### 각 VPC 라우팅 설정
- **VPC-A route table**
  Destination | Target
  --- | ---
  10.10.0.0/16 | local
  0.0.0.0/0 | igw
  10.0.0.0/8 | tgw
- **VPC-B route table**
  Destination | Target
  --- | ---
  10.20.0.0/16 | local
  10.0.0.0/8 | tgw
- **VPC-C route table**
  Destination | Target
  --- | ---
  10.30.0.0/16 | local
  10.0.0.0/8 | tgw

<hr>

## 참고
- **Transit Gateway 작동 원리** - https://docs.aws.amazon.com/vpc/latest/tgw/how-transit-gateways-work.html
