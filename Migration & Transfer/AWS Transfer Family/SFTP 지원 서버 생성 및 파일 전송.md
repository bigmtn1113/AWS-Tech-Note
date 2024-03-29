# SFTP 지원 서버 생성 및 파일 전송

<br>

## 사용 방식
- 활성화 프로토콜 　 - **SFTP**  
- 자격 증명 공급자 　- **Service Managed**  
- 엔드포인트 유형 　 - **공개적으로 액세스 가능**  
- 도메인 　　　 　　 - **Amazon S3**  
- 사용자 인증 방식　 - **SSH Key Pair**  

<br>

## Architecture 참고
![trasfer-for-sftp-endpoint-types-202004-01](https://user-images.githubusercontent.com/46125158/167397449-ed8122df-47ea-402d-b0c9-d574dd0ead71.png)

<br>

## 사전 작업
- [IAM, S3 Bucket, SSH Key Pair 생성](https://github.com/bigmtn1113/AWS-Tech-Note/blob/master/Migration%20%26%20Transfer/AWS%20Transfer%20Family/%EC%82%AC%EC%A0%84%20%EC%9E%91%EC%97%85.md)
- **Endpoint에 적용할 VPC default 보안 그룹 설정(SFTP Port)**  
  ![Cap 2022-05-08 20-02-12-109](https://user-images.githubusercontent.com/46125158/167293256-25846871-88fe-4b65-bac4-da9093fef9e7.png)

<hr>

## 서버 생성
![Cap 2022-05-05 16-54-28-907](https://user-images.githubusercontent.com/46125158/166897871-2f555767-1f7f-4102-a00c-7ab3327d3f74.png)
![Cap 2022-05-05 16-54-56-603](https://user-images.githubusercontent.com/46125158/166897875-f4686e70-802e-46a4-94f2-53f89360fdf4.png)
![Cap 2022-05-05 16-55-18-570](https://user-images.githubusercontent.com/46125158/166897876-bbe5cf51-ecc1-4dab-99f1-7b56a293d8a0.png)
![Cap 2022-05-05 16-55-28-284](https://user-images.githubusercontent.com/46125158/166897878-f74960ed-288e-4bf5-8214-ee207181e4d7.png)
![Cap 2022-05-05 16-55-40-527](https://user-images.githubusercontent.com/46125158/166897879-b2a4e517-793f-43c6-837e-75029a893050.png)
![Cap 2022-05-05 16-57-50-762](https://user-images.githubusercontent.com/46125158/166897881-5278d851-d7f4-4a6f-9105-9da34153e623.png)
![Cap 2022-05-05 17-02-11-738](https://user-images.githubusercontent.com/46125158/166897882-2faa1425-3ef1-43fa-a425-2e51736c614f.png)

<br>

## Amazon S3 서비스 관리 사용자 추가
![Cap 2022-05-05 17-13-09-478](https://user-images.githubusercontent.com/46125158/166905417-6b840b42-9306-4a8e-b824-27eb8f4850a0.png)
![Cap 2022-05-05 17-43-25-985](https://user-images.githubusercontent.com/46125158/166905424-c4812b27-cbaa-4134-a6be-e1e227286059.png)

<br>

## FileZilla를 사용하여 파일 전송
![Cap 2022-05-05 17-45-48-621](https://user-images.githubusercontent.com/46125158/166904124-5db8da09-7ff2-45d6-97f2-3528fd4f4a8d.png)
![Cap 2022-05-05 17-46-42-987](https://user-images.githubusercontent.com/46125158/166904132-94948153-3114-43b2-8372-cd59bac7b58d.png)
![Cap 2022-05-05 17-50-19-737](https://user-images.githubusercontent.com/46125158/166904138-c7e71686-97d3-455f-89c0-2daf3a8ee2d4.png)

<hr>

## 참고
- **Architecture** - https://dev.classmethod.jp/articles/trasfer-for-sftp-endpoint-types-202004/
- **SFTP 지원 서버 생성** - https://docs.aws.amazon.com/ko_kr/transfer/latest/userguide/create-server-sftp.html
- **S3 서비스 관리 사용자 추가** - https://docs.aws.amazon.com/ko_kr/transfer/latest/userguide/service-managed-users.html#add-s3-user
- **FileZilla 사용** - https://docs.aws.amazon.com/ko_kr/transfer/latest/userguide/transfer-file.html#filezilla
