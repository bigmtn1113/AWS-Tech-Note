# AWS Site-to-Site VPN - Virtual Private Gateway 사용

<br>

## 시나리오
**AWS Site-to-Site VPN**를 이용해 VPC(10.0.0.0/16)의 private subnet에 있는 EC2 인스턴스와 ON-PREMISE(192.168.0.0/16)의 private subnet에 있는 On-Premise EC2 인스턴스 간 통신

<br>

## Architect
![image](https://user-images.githubusercontent.com/46125158/210130291-df6f02f4-195b-4fb5-b2f7-52f7c658924d.png)

<br>

## AWS Site-to-Site VPN
여기선 온프레미스 환경이 VPC로 구성되어 있지만 VPN 연결은 VPC와 자체 온프레미스 네트워크 간의 연결을 의미  
Site-to-Site VPN은 인터넷 프로토콜 보안(IPSec) VPN 연결을 지원

### 개념
#### VPN connection
온프레미스 장비와 VPC 간의 보안 연결

#### VPN tunnel
데이터를 고객 네트워크에서 AWS와 주고받을 수 있는 암호화된 링크  
각 VPN 연결에는 고가용성을 위해 동시에 사용할 수 있는 두 개의 VPN 터널이 존재

#### Customer gateway
Customer gateway device에 대한 정보를 AWS에 제공하는 AWS 리소스  
ex) Customer gateway device IP(3.38.166.49)

#### Customer gateway device
Site-to-Site VPN 연결을 위해 고객 측에 설치된 물리적 디바이스 또는 소프트웨어 애플리케이션  
여기선 EC2 위에 Openswan을 설치함으로써 구현

#### Virtual private gateway
단일 VPC에 연결할 수 있는 Site-to-Site VPN 연결의 Amazon 측 VPN 엔드포인트

<hr>

## 참고
- **AWS Site-to-Site VPN란?** - https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html
