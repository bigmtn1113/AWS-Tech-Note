# VPC Endpoint Interface 및 VPC Endpoint Service 사용 - AWS PrivateLink

<br>

## 시나리오
VPC(10.0.0.0/16)에 있는 EC2 인스턴스에서 **VPC Endpoint Interface 및 VPC Endpoint Service**를 이용해  
VPC(192.168.0.0/16)의 private subnet에 있는 EC2 WEB 인스턴스로 접근

<br>

## Architect
![image](https://user-images.githubusercontent.com/46125158/208239555-9c0f29b1-6abc-4a67-b5e3-c6e3eb52653a.png)

<br>

## AWS PrivateLink 개념
AWS PrivateLink는 VPC의 리소스가 private IP를 사용하여 다른 VPC의 서비스에 연결할 수 있도록 허용

### 서비스 제공자
EC2 인스턴스와 같은 AWS 리소스를 사용하거나 온프레미스 서버를 사용하여 서비스 호스팅 가능

#### Endpoint services
서비스 공급자는 endpoint service를 생성할 때 로드 밸런서를 지정  
로드 밸런서는 서비스 소비자로부터 요청을 받아 서비스로 라우팅

#### Service names
각 endpoint service는 서비스 이름으로 식별  
서비스 소비자는 VPC endpoint를 생성할 때 서비스 이름을 지정해야 하므로 서비스 제공자는 서비스 소비자와 서비스 이름 공유 필요

### 서비스 소비자
EC2 인스턴스와 같은 AWS 리소스 또는 온프레미스 서버에서 endpoint service에 액세스 가능

#### VPC endpoints
서비스 소비자는 VPC endpoint를 생성하여 VPC를 endpoint service에 연결  
서비스 소비자는 VPC endpoint를 생성할 때 endpoint service의 service name 지정  
여러 유형의 VPC endpoint가 있으나 여기선 Interface만 소개

- Interface
  - NLB를 사용하여 트래픽을 분산하는 endpoint service로 트래픽을 전송하는 interface enpoint를 생성
  - Endpoint service로 향하는 트래픽은 DNS를 사용하여 확인

#### Endpoint network interfaces
Endpoint service로 향하는 트래픽의 진입점 역할을 하는 요청자 관리 네트워크 인터페이스  
VPC endpoint를 생성할 때 지정하는 각 서브넷에 대해 서브넷에 endpoint network interface를 생성

### AWS PrivateLink 연결
VPC의 트래픽은 VPC endpoint와 endpoint service 간의 연결을 사용하여 endpoint service로 전송  
VPC endpoint와 endpoint service 간의 트래픽은 암호화되어 공용 인터넷을 통과하지 않고 AWS 네트워크 내에 유지

<hr>

## 참고
- **AWS PrivateLink 개념** - https://docs.aws.amazon.com/vpc/latest/privatelink/concepts.html
- **AWS PrivateLink에서 제공하는 서비스 생성** - https://docs.aws.amazon.com/vpc/latest/privatelink/create-endpoint-service.html
