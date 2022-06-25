# Amazon WorkSpaces 생성

<br/>

## Architecture 참고
![Image](https://user-images.githubusercontent.com/46125158/170857203-b93be72b-6854-4746-8ea4-248c125078f9.png)

<hr>

## 사전 작업
### [AWS Managed Microsoft Active Directory(AD) 생성](https://github.com/kva231/AWS-Tech-Note/blob/master/Security%2C%20Identity%2C%20%26%20Compliance/AWS%20Directory%20Service/AWS%20Managed%20Microsoft%20Active%20Directory(AD)%20%EC%83%9D%EC%84%B1.md)

<hr>

## WorkSpace 생성
![Cap 2022-05-29 17-50-00-180](https://user-images.githubusercontent.com/46125158/170989803-15a5ac6b-7e6e-4c06-a0cc-ad4488c55389.png)  
![Cap 2022-05-29 17-50-27-923](https://user-images.githubusercontent.com/46125158/170989849-278b6f9b-321b-4967-8a28-1885c28680ac.png)  

### 디렉터리 및 서브넷 선택
![Cap 2022-05-29 17-51-26-558](https://user-images.githubusercontent.com/46125158/170990040-e7bfc444-0af8-4703-b675-c34f5aab67a8.png)  

### 디렉터리에 새 사용자 추가
![Cap 2022-05-29 17-53-06-399](https://user-images.githubusercontent.com/46125158/170990073-b87512d2-6f5b-4c7c-a436-ec11139fb670.png)  
![Cap 2022-05-29 17-53-35-314](https://user-images.githubusercontent.com/46125158/170990082-3ef4216c-2eb9-466d-8939-d4a481f3041e.png)  

### 번들 선택
![Cap 2022-05-29 17-54-30-654](https://user-images.githubusercontent.com/46125158/170990114-53ee8948-537e-4e65-92e1-c769a3ad84d0.png)  

### 실행 모드 선택
![Cap 2022-05-29 17-55-09-994](https://user-images.githubusercontent.com/46125158/170990150-7b5be1ad-54a2-42b3-8356-ffd56e320acc.png)  

### 설정 내용 확인
![Cap 2022-05-29 17-55-25-908](https://user-images.githubusercontent.com/46125158/170990172-b77efe52-3b78-4214-a390-1f1ab7d57a98.png)  

### 생성 완료
![Cap 2022-05-29 18-13-06-716](https://user-images.githubusercontent.com/46125158/170990189-742176b8-120f-4407-8efe-6fb402c1e4e9.png)  

<hr>

## WorkSpace 연결
### WorkSpace 생성할 때 지정한 사용자의 이메일 확인
![Cap 2022-05-29 18-16-29-538](https://user-images.githubusercontent.com/46125158/170991529-17d2ca58-e0e9-4bf3-8b16-da81c187c1a4.png)  

### 사용자 프로필 작성
![Cap 2022-05-29 18-17-24-623](https://user-images.githubusercontent.com/46125158/170991554-606e3bec-7289-4d52-9f71-b98998f93ae3.png)  

### WorkSpaces 클라이언트 다운로드 및 설치
![Cap 2022-05-29 18-18-04-859](https://user-images.githubusercontent.com/46125158/170991597-f5056584-9289-413e-bb11-dda26922625b.png)  
![Cap 2022-05-29 18-19-04-950](https://user-images.githubusercontent.com/46125158/170991628-1e4dd4b9-7b94-4762-a74e-315c7144e56b.png)  

### WorkSpace 접속
![Cap 2022-05-29 18-22-33-939](https://user-images.githubusercontent.com/46125158/170991665-04e23fb1-cc5d-4e85-b28a-2ce7e9b55eb1.png)  
![Cap 2022-05-29 18-24-32-453](https://user-images.githubusercontent.com/46125158/170991678-7e9ede48-b7a8-4525-a4b5-414677eedb1c.png)  
![Cap 2022-05-29 18-24-59-198](https://user-images.githubusercontent.com/46125158/170991684-24a005de-2b14-40bb-a1a4-7cd7c5449b28.png)  

### WorkSpace 접속 확인
![Cap 2022-05-29 18-26-04-879](https://user-images.githubusercontent.com/46125158/170991706-75542c9f-1811-48e6-aa71-a7d6467531af.png)  

<hr>

## 참고
- **AWS Managed Microsoft AD를 사용하여 WorkSpace 시작** - https://docs.aws.amazon.com/workspaces/latest/adminguide/launch-workspace-microsoft-ad.html
- **WorkSpaces 보안 그룹** - https://docs.aws.amazon.com/workspaces/latest/adminguide/amazon-workspaces-security-groups.html
