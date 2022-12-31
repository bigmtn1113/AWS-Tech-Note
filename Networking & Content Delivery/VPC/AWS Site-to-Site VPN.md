# AWS Site-to-Site VPN - Virtual Private Gateway 사용

<br>

## 시나리오
**AWS Site-to-Site VPN**를 이용해 VPC(10.0.0.0/16)의 private subnet에 있는 EC2 인스턴스와 ON-PREMISE(192.168.0.0/16)의 private subnet에 있는 On-Premise EC2 인스턴스 간 통신

<br>

## Architect
![image](https://user-images.githubusercontent.com/46125158/210130291-df6f02f4-195b-4fb5-b2f7-52f7c658924d.png)

<br>

## AWS Site-to-Site VPN란?
여기선 온프레미스 환경이 VPC로 구성되어 있지만 VPN 연결은 VPC와 자체 온프레미스 네트워크 간의 연결을 의미  
Site-to-Site VPN은 인터넷 프로토콜 보안(IPSec) VPN 연결을 지원

### 개념
#### VPN connection
온프레미스 장비와 VPC 간의 보안 연결

#### VPN tunnel
데이터를 고객 네트워크에서 AWS와 주고받을 수 있는 암호화된 링크

각 VPN 연결에는 고가용성을 위해 동시에 사용할 수 있는 두 개의 VPN 터널이 존재하며 각 터널은 고유의 퍼블릭 IP 주소를 사용  
ex) 3.34.162.95, 52.78.183.110

터널 하나가 사용 불가능하게 되면 네트워크 트래픽은 해당 특정 Site-to-Site VPN 연결에 사용 가능한 터널로 자동으로 라우팅

#### Customer gateway
Customer gateway device에 대한 정보를 AWS에 제공하는 AWS 리소스  
ex) Customer gateway device IP(3.38.166.49)

#### Customer gateway device
Site-to-Site VPN 연결을 위해 고객 측에 설치된 물리적 디바이스 또는 소프트웨어 애플리케이션  
여기선 EC2 위에 Openswan을 설치함으로써 구현

#### Virtual private gateway
단일 VPC에 연결할 수 있는 Site-to-Site VPN 연결의 Amazon 측 VPN 엔드포인트

Virtual private gateway를 생성할 때 Amazon 측 게이트웨이의 프라이빗 Autonomous System Number(ASN) 지정 가능  
지정하지 않는 경우 default 값은 64512

<br>

## Customer gateway device EC2 인스턴스 설정 시 주의사항
### Openswan 설치
Customer gateway device EC2 인스턴스에 접속 후 진행

```bash
$ sudo yum -y install openswan
```

### Customer gateway device에 특정한 configuration 파일대로 인스턴스 설정
`/etc/ipsec.d/aws.conf` 파일 설정 시 주의해야 할 점
- `auth=esp` 부분 주석 처리 혹은 삭제
- `leftsubnet`은 온프레미스 네트워크로 설정
  - ex) 192.168.0.0/16(ON-PREMISE)
- `rightsubnet`은 원격 네트워크로 설정
  - ex) 10.0.0.0/16(VPC)
- `overlapip=yes` 옵션 추가
  - 동일한 IP(3.38.166.49 - Customer gateway device IP)를 사용하기 위한 목적. leftid 확인
  - 이 부분을 추가하지 않으면 Tunnel이 하나만 Up

```bash
conn Tunnel1
	leftid=3.38.166.49
	right=3.34.162.95
  ...
	#auth=esp
	...
	leftsubnet=192.168.0.0/16
	rightsubnet=10.0.0.0/16
  ...
  overlapip=yes

conn Tunnel2
	leftid=3.38.166.49
	right=52.78.183.110
  ...
	#auth=esp
	...
	leftsubnet=192.168.0.0/16
	rightsubnet=10.0.0.0/16
  ...
  overlapip=yes
```

※ configuration에 명시된 대로 구성이 끝나면 `sudo systemctl restart ipsec.service` 명령어 실행

### Disable Source / destination check 설정
1. EC2 인스턴스를 선택하고 **Actions, Networking, Change source/destination check**를 선택
2. **Source / destination checking**에서 **Stop** 선택 후, 저장

각 EC2 인스턴스는 기본적으로 소스/대상 확인을 수행  
즉, 인스턴스는 전송하거나 수신하는 모든 트래픽의 소스 또는 대상  
그러나 NAT/VPN 인스턴스는 트래픽의 소스도 대상도 아니며 트래픽의 게이트웨이 역할만 수행  
따라서 NAT/VPN 인스턴스에서 소스/대상 확인을 비활성화로 설정하는 작업 필요

비활성화하지 않으면 NAT/VPN 인스턴스 자신이 트래픽의 소스 또는 대상이 아닐 경우 트래픽을 차단

### Security group 설정
IPSec(VPN 터널링)에서 openswan은 udp 4500 포트를 NAT를 통과할 때 사용

<hr>

## 참고
- **AWS Site-to-Site VPN란?** - https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html
- **AWS Site-to-Site VPN 작동 방식** - https://docs.aws.amazon.com/ko_kr/vpn/latest/s2svpn/how_it_works.html
- **Site-to-Site VPN 연결의 터널 옵션** - https://docs.aws.amazon.com/vpn/latest/s2svpn/VPNTunnels.html
- **Disable Source / destination checks** - https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html#EIP_Disable_SrcDestCheck
