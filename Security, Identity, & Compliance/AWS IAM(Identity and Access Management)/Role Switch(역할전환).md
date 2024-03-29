# Role Switch(역할전환)

<br>

## Architecture 참고
![Image](https://user-images.githubusercontent.com/46125158/178145658-89f766ca-f095-4fba-b22e-afe8701ada13.png)

<br>

## 사전 작업

- **AWS Account 두 개(Dev. Prod) 준비**
- **Dev Account User 준비**

<hr/>

## Role 생성
### Dev Account ID 및 User 확인
![Cap 2022-07-10 15-32-06-475](https://user-images.githubusercontent.com/46125158/178134354-547f48ce-df3a-4eae-ba46-f4a0bc632cbd.png)

### Prod Account에서 Role 생성
- **신뢰할 수 있는 엔터티 선택**  
  MFA 설정을 여기선 생략했으나 설정하는 것을 권장  
  ![Cap 2022-07-10 15-35-01-609](https://user-images.githubusercontent.com/46125158/178135702-7184effd-688e-4d70-b183-2fc1496f6af8.png)

- **권한 추가**  
  ![Cap 2022-07-10 15-35-41-478](https://user-images.githubusercontent.com/46125158/178135714-d707e4ff-ffc9-465a-a9ed-39620671e125.png)

- **이름 지정, 검토 및 생성**  
  ![Cap 2022-07-10 15-37-08-355](https://user-images.githubusercontent.com/46125158/178135733-6e995af1-fa32-4e1e-9682-2762335d33a4.png)

<br>

## Role 편집
### 생성한 Role 확인
![Cap 2022-07-10 15-40-39-731](https://user-images.githubusercontent.com/46125158/178135758-70bd03fc-5ba2-445d-8436-978eb51e9fae.png)

### 생성한 Role 선택 후, 정책 편집
![Cap 2022-07-10 15-41-23-524](https://user-images.githubusercontent.com/46125158/178135768-d09e8b99-2263-4f41-801c-c144d5a92358.png)

<br>

## Role Switch
### Dev Account user login 후, Switch role 클릭
![Cap 2022-07-10 15-42-57-587](https://user-images.githubusercontent.com/46125158/178135796-5f3ab2d9-bb24-49fd-a776-4dc2fedc3fe1.png)

### Role Switch 실행
![Cap 2022-07-10 15-44-14-564](https://user-images.githubusercontent.com/46125158/178135812-bdc5485f-b663-484f-8bc9-71e4ec97e851.png)

### 확인
![Cap 2022-07-10 15-44-37-845](https://user-images.githubusercontent.com/46125158/178135821-b38dde62-366d-4b3b-9f63-6b878a48434c.png)

<br>

## AWS CLI 이용
### aws configure 실행
**AWS CLI는 민감한 자격 증명 정보를 홈 디렉터리의 .aws라는 폴더에 있는 credentials라는 로컬 파일에 저장**  
**덜 민감한 구성 옵션은 홈 디렉터리의 .aws 폴더에 있는 config라는 로컬 파일에 저장됨**  
![Cap 2022-07-10 21-26-23-591](https://user-images.githubusercontent.com/46125158/178145407-ffee58d6-7e34-4530-9644-49dcc53ed720.png)

### ~/.aws/credentials 내용 확인
![Cap 2022-07-10 21-25-27-963](https://user-images.githubusercontent.com/46125158/178145449-50299961-147f-43ba-96f7-61aa9c991b3d.png)

### ~/.aws/config 편집
**Prod Account에서 생성한 Role arn 등록**  
![Cap 2022-07-10 21-27-24-440](https://user-images.githubusercontent.com/46125158/178145193-d2dd5ac1-9a9f-4ed0-a69b-ff3015f41f28.png)

### 확인
**Prod Account S3 목록 확인 가능**  
![Cap 2022-07-10 21-28-41-734](https://user-images.githubusercontent.com/46125158/178145233-304a08cc-7359-4bd7-9cb6-6b8e92cdc1f0.png)

<br>

## ※ AWS Extend Switch Roles 사용
### AWS Extend Switch Roles Chrome에 추가
![Cap 2022-07-10 16-52-31-724](https://user-images.githubusercontent.com/46125158/178136596-cadc53a5-c684-41a2-9444-5fa3841d7157.png)  
![Cap 2022-07-10 16-52-55-636](https://user-images.githubusercontent.com/46125158/178136600-d89cf61b-2027-47c4-b94f-6287c95e3d1a.png)  
![Cap 2022-07-10 16-53-38-385](https://user-images.githubusercontent.com/46125158/178136605-b8f18f66-9e32-4aa7-b71b-c57e1a72d689.png)

### Configuration 작성
**AWS Extend Switch Roles 실행**  
![Cap 2022-07-10 16-54-15-667](https://user-images.githubusercontent.com/46125158/178136639-fe7d31a2-ea92-46ef-a117-063a82448826.png)  
![Cap 2022-07-10 16-56-01-542](https://user-images.githubusercontent.com/46125158/178136642-2d52da1c-134a-44d6-86f5-533cd0caf809.png)

### Role Switch 실행
![Cap 2022-07-10 16-56-48-458](https://user-images.githubusercontent.com/46125158/178136649-e8815c74-3088-4e10-a73a-a7b2d8d28579.png)

### 확인
![Cap 2022-07-10 16-57-22-354](https://user-images.githubusercontent.com/46125158/178136654-9821ce09-4d29-4221-9a87-1014effb8923.png)

<hr>

## 참고
- **아키텍처 참고** - https://aws.amazon.com/ko/blogs/security/aws-cloudtrail-now-tracks-cross-account-activity-to-its-origin/
- **구성 설정이 저장되는 장소** - https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html#cli-configure-files-where
- **AWS Extend Switch Roles GitHub** - https://github.com/tilfinltd/aws-extend-switch-roles
