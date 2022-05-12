# AWS Client VPN을 이용한 Private 리소스 접근

<br/>

## Architecture 참고
![image](https://user-images.githubusercontent.com/46125158/167408967-c4cf7081-de67-4349-bc6c-7f6e74e5adcf.png)

<hr>

## 사전 작업
- [서버 및 클라이언트 인증서와 키를 생성 후, ACM 업로드](https://github.com/kva231/AWS-Tech-Note/blob/master/Security%2C%20Identity%2C%20%26%20Compliance/AWS%20Certificate%20Manager/%EC%84%9C%EB%B2%84%20%EB%B0%8F%20%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8%20%EC%9D%B8%EC%A6%9D%EC%84%9C%EC%99%80%20%ED%82%A4%EB%A5%BC%20%EC%83%9D%EC%84%B1%20%ED%9B%84%2C%20ACM%20%EC%97%85%EB%A1%9C%EB%93%9C.md)
- Private Subnet에 접속 테스트용 EC2 생성

<hr>

## Client VPN Endpoint 생성
![Cap 2022-05-12 20-29-28-063](https://user-images.githubusercontent.com/46125158/168066709-2908e74f-ebfd-4e67-b8ea-57c5f5afcf1b.png)  
![Cap 2022-05-12 20-32-07-979](https://user-images.githubusercontent.com/46125158/168068008-59cc4093-cd9a-4ab0-a509-76c75ada135c.png)  
![Cap 2022-05-12 20-48-41-611](https://user-images.githubusercontent.com/46125158/168068363-71df11e3-2686-44f2-acdc-af9ab0b95456.png)

<hr>

## Client VPN Endpoint 설정
- 대상 네트워크 연결  
  ![Cap 2022-05-12 20-52-53-635](https://user-images.githubusercontent.com/46125158/168070088-0a243586-6d79-4fb3-8b52-5c99d784753c.png)  
  ![Cap 2022-05-12 20-54-17-681](https://user-images.githubusercontent.com/46125158/168070102-8bdf5069-266d-4d83-bef1-9fb03278124f.png)  
  ![Cap 2022-05-12 21-05-05-674](https://user-images.githubusercontent.com/46125158/168075999-b96968e9-57b7-45a4-a16a-26338011b077.png)  
  ![Cap 2022-05-12 21-01-58-765](https://user-images.githubusercontent.com/46125158/168070341-2a1ff467-53c7-4a74-aacf-e980393b5a48.png)  

- 권한 부여 규칙 추가  
  클라이언트가 VPC에 액세스하려면 Client VPN 엔드포인트의 라우팅 테이블에 VPC로 연결되는 경로가 있고 권한 부여 규칙이 있어야 함  
  경로는 자동으로 추가됨  
  ![Cap 2022-05-12 21-12-10-155](https://user-images.githubusercontent.com/46125158/168075349-0c029fa3-3615-4c80-8fbd-80adec96694c.png)
  ![Cap 2022-05-12 21-20-12-061](https://user-images.githubusercontent.com/46125158/168073787-941ffc91-0f6a-4d69-8bac-bcaf3ba597ff.png)  
  ![Cap 2022-05-12 21-21-24-096](https://user-images.githubusercontent.com/46125158/168075666-4c6e79b2-a563-4417-a82f-7b0e1c4a20e2.png)  
  ![Cap 2022-05-12 21-22-33-519](https://user-images.githubusercontent.com/46125158/168075773-af68c11d-35cd-452d-ab17-6813f17d3b10.png)

<hr>

## Network interfaces 확인
가용성을 위해 Associate한 subnet 당 약 2개의 ENI가 생성됨  
![Cap 2022-05-12 22-31-45-839](https://user-images.githubusercontent.com/46125158/168087054-676afc42-b1b4-4f6f-bd88-5db366c843bc.png)

<hr>

## 보안 그룹 설정
- Client VPN 엔드포인트에 적용된 보안 그룹이 인터넷으로의 아웃바운드 트래픽을 허용해야 인터넷 접속이 가능함  
  ![Cap 2022-05-12 21-56-14-288](https://user-images.githubusercontent.com/46125158/168079827-73210ae6-3c6c-4da4-8a41-50c66f7ca178.png)

- Private Subnet에 있는 EC2 인스턴스 보안 그룹 인바운드 설정
  ![Cap 2022-05-12 22-37-07-005](https://user-images.githubusercontent.com/46125158/168088218-08eefd4d-8b21-4133-ba9d-f7a7ca795d8d.png)


<hr>

## VPN 클라이언트 애플리케이션 설치
[AWS Client VPN 다운로드](http://aws.amazon.com/vpn/client-vpn-download/)  
![Cap 2022-05-12 22-01-44-816](https://user-images.githubusercontent.com/46125158/168080746-0bf52772-0780-4a14-8fd2-9d56b9c77db7.png)

<hr>

## Client VPN 엔드포인트 구성 파일 다운로드 및 수정
![Cap 2022-05-12 21-59-52-571](https://user-images.githubusercontent.com/46125158/168080366-9efe21ab-5100-45b2-9f3c-cfcfebc07ab2.png)

- downloaded-client-config.ovpn 파일 내용 조회
  ![Cap 2022-05-12 22-05-28-809](https://user-images.githubusercontent.com/46125158/168081526-d52b66e5-6e67-4dfd-82f8-1e3a59c71b34.png)

- downloaded-client-config.ovpn 파일 내용 수정(ca 밑에 cert와 key 내용 추가)  
  ![Cap 2022-05-12 22-12-42-247](https://user-images.githubusercontent.com/46125158/168084273-d0d8c76c-79bf-482d-904a-fd91e97a32d2.png)  
  ![Cap 2022-05-12 22-13-00-282](https://user-images.githubusercontent.com/46125158/168084286-7ef4ef64-8022-4843-bff0-85e4389e34ba.png)

<hr>

## AWS VPN Client 설정 및 사용
- 프로그램 실행  
  ![Cap 2022-05-12 22-23-46-924](https://user-images.githubusercontent.com/46125158/168089112-642f358e-f6f6-4d16-8fcb-69c1ee15cba5.png)  
  ![Cap 2022-05-12 22-24-16-142](https://user-images.githubusercontent.com/46125158/168089185-23c3f5d0-2ccf-4bc3-993f-2ba3b2c357f8.png)

- ovpn 파일 등록  
  ![Cap 2022-05-12 22-25-02-223](https://user-images.githubusercontent.com/46125158/168089394-3a47dd05-192f-46c2-b571-f0663a705627.png)  
  ![Cap 2022-05-12 22-25-52-023](https://user-images.githubusercontent.com/46125158/168089453-c011830d-18b8-439f-96ea-80aa8d3e4b08.png)  
  ![Cap 2022-05-12 22-26-37-103](https://user-images.githubusercontent.com/46125158/168090045-be1c031e-9ed8-4458-accc-b2382bf27b12.png)  
  ![Cap 2022-05-12 22-26-47-485](https://user-images.githubusercontent.com/46125158/168089551-69fd5964-2f73-4a70-beaa-ad359a0e1b0a.png)  
  ![Cap 2022-05-12 22-27-13-782](https://user-images.githubusercontent.com/46125158/168089599-10180c97-aebe-43a2-a533-eaad3cc395fa.png)  
  ![Cap 2022-05-12 22-28-12-596](https://user-images.githubusercontent.com/46125158/168089887-9bfaaecb-30df-4702-a3dd-39fe1a305d29.png)

<hr>

## Private 리소스 접근 테스트
- AWS VPN Client 실행 후, Private Subnet에 있는 EC2 인스턴스 접속
![Cap 2022-05-12 22-55-55-275](https://user-images.githubusercontent.com/46125158/168092133-9c522598-dad5-4dbb-8cb2-84da55a462d6.png)

※ Split-tunnel 기능을 활성화했으므로 인터넷 접속도 가능  
활성화 하지 않았다면 Client VPN Endpoint에 0.0.0.0/0에 대한 라우팅 테이블 추가 작업 필요

<hr>

## 참고
- **Client VPN 시작하기** - https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/cvpn-getting-started.html
- **Split-tunnel** - https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/split-tunnel-vpn.html
- **Client VPN 사용 설명서** - https://docs.aws.amazon.com/ko_kr/vpn/latest/clientvpn-user/user-getting-started.html
