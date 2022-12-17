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


<hr>

## 참고
- **AWS PrivateLink 개념** - https://docs.aws.amazon.com/vpc/latest/privatelink/concepts.html
- **AWS PrivateLink에서 제공하는 서비스 생성** - https://docs.aws.amazon.com/vpc/latest/privatelink/create-endpoint-service.html
