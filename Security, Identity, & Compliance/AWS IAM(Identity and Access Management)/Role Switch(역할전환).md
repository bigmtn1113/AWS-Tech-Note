# Role Switch(역할전환)

<br/>

## Architecture 참고
![Image](https://user-images.githubusercontent.com/46125158/178133881-575daa14-8fb5-4981-a20e-0febd0e99fbb.png)

<hr>

## 사전 작업

### AWS Account 두 개(Dev. Prod) 준비

### Dev Account User 준비

<hr>

## Role 생성
### Dev Account ID 및 User 확인
![Cap 2022-07-10 15-32-06-475](https://user-images.githubusercontent.com/46125158/178134354-547f48ce-df3a-4eae-ba46-f4a0bc632cbd.png)

### Prod Account에서 Role 생성
- **신뢰할 수 있는 엔터티 선택**  
  ![Cap 2022-07-10 15-35-01-609](https://user-images.githubusercontent.com/46125158/178135702-7184effd-688e-4d70-b183-2fc1496f6af8.png)

- **권한 추가**  
  ![Cap 2022-07-10 15-35-41-478](https://user-images.githubusercontent.com/46125158/178135714-d707e4ff-ffc9-465a-a9ed-39620671e125.png)

- **이름 지정, 검토 및 생성**  
  ![Cap 2022-07-10 15-37-08-355](https://user-images.githubusercontent.com/46125158/178135733-6e995af1-fa32-4e1e-9682-2762335d33a4.png)

<hr>

## Role 편집
### 생성한 Role 확인
![Cap 2022-07-10 15-40-39-731](https://user-images.githubusercontent.com/46125158/178135758-70bd03fc-5ba2-445d-8436-978eb51e9fae.png)

### 생성한 Role 선택 후, 정책 편집
![Cap 2022-07-10 15-41-23-524](https://user-images.githubusercontent.com/46125158/178135768-d09e8b99-2263-4f41-801c-c144d5a92358.png)

<hr>

## Role Switch
### Dev Account user login 후, Switch role 클릭
![Cap 2022-07-10 15-42-57-587](https://user-images.githubusercontent.com/46125158/178135796-5f3ab2d9-bb24-49fd-a776-4dc2fedc3fe1.png)

### Role Switch 실행
![Cap 2022-07-10 15-44-14-564](https://user-images.githubusercontent.com/46125158/178135812-bdc5485f-b663-484f-8bc9-71e4ec97e851.png)

### 확인
![Cap 2022-07-10 15-44-37-845](https://user-images.githubusercontent.com/46125158/178135821-b38dde62-366d-4b3b-9f63-6b878a48434c.png)

<hr>

## 참고
- **아키텍처 참고** - https://aws.amazon.com/ko/blogs/security/aws-cloudtrail-now-tracks-cross-account-activity-to-its-origin/
