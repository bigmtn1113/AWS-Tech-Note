# AWS Client VPN

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
  ![Cap 2022-05-12 20-55-27-210](https://user-images.githubusercontent.com/46125158/168070106-e640c14e-87d4-4970-abbc-1b22a6de7868.png)



<hr>

## 참고
- **Client VPN 시작하기** - https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/cvpn-getting-started.html
- **Split-tunnel** - https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/split-tunnel-vpn.html
