# AWS Managed Microsoft Active Directory(AD) 생성

<br/>

## Architecture 참고
![image](https://user-images.githubusercontent.com/46125158/168287819-996f6ba8-7f6a-4140-8501-5cc375635ba2.png)

<hr>

## 사전 작업
- **VPC에 Private Subnet 2개 생성**  
  ※ 최소 2개의 서브넷. 각 서브넷은 서로 다른 가용 영역에 존재해야 함

<hr>

## Directory 생성
![Cap 2022-05-13 20-47-47-559](https://user-images.githubusercontent.com/46125158/168277977-4a04a376-2cf0-4e3a-9fc2-cbf708a31a0a.png)  
![Cap 2022-05-13 20-49-34-221](https://user-images.githubusercontent.com/46125158/168279090-80a8cbd3-f3b0-4033-a3f4-9d8f6754bea7.png)  
![Cap 2022-05-13 20-51-07-997](https://user-images.githubusercontent.com/46125158/168279098-b6a2efb3-7111-46ad-99ab-4f98e799f42d.png)  
![Cap 2022-05-13 20-51-56-195](https://user-images.githubusercontent.com/46125158/168279104-21cea3e9-9f7e-4abd-835c-b2b7a9f70ff6.png)  
![Cap 2022-05-13 20-52-42-245](https://user-images.githubusercontent.com/46125158/168279116-8f06076e-7cb7-45f4-b8ff-570f9a17b016.png)

※ **디렉터리 생성까지 20~40분 시간 소요**  
![Cap 2022-05-13 21-15-18-700](https://user-images.githubusercontent.com/46125158/168281137-1f951242-5537-402a-ac96-e9f3abf3eb6c.png)  
![Cap 2022-05-13 21-17-16-110](https://user-images.githubusercontent.com/46125158/168281601-d33a2872-e9de-4b70-9c28-e832cd0fb6a4.png)

<hr>

## 보안 그룹 확인
※ **사용자 AWS 관리형 Microsoft AD와 통신할 수 있는 유일한 인바운드 트래픽는 로컬 VPC 및 VPC 라우팅 트래픽**  
도메인 컨트롤러에 대한 트래픽이 VPC, 다른 피어링된 VPC 또는 AWS Direct Connect, AWS Transit Gateway 또는 가상 프라이빗 네트워크를 사용하여 
연결한 네트워크에서의 트래픽으로 제한되므로 0.0.0.0/0 규칙은 보안 취약성을 발생시키지 않음

![Cap 2022-05-13 21-21-50-193](https://user-images.githubusercontent.com/46125158/168283489-bc84786b-f0c1-4060-9f43-a0f43d4be6e2.png)  
![Cap 2022-05-13 21-23-57-194](https://user-images.githubusercontent.com/46125158/168283713-ff941c8e-9110-4a1b-8012-121b21d06557.png)  
![Cap 2022-05-13 21-24-14-809](https://user-images.githubusercontent.com/46125158/168283719-f4371a18-e7c5-4721-8c2c-2f08e793d7b2.png)

<hr>

## Network Interfaces 확인
![Cap 2022-05-13 21-35-16-737](https://user-images.githubusercontent.com/46125158/168284498-098edd29-2abd-44d0-8d3a-8374cdcfc1b6.png)

<hr>

## 참고
- **AWS Managed Microsoft AD 사전 요구 사항** - https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_getting_started_prereqs.html
- **디렉터리 생성 후, 만들어지는 요소** - https://docs.aws.amazon.com/directoryservice/latest/admin-guide/ms_ad_getting_started_what_gets_created.html
