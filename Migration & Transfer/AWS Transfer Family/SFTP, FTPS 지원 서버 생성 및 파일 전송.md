# SFTP, FTPS 지원 서버 생성 및 파일 전송

<br/>

## 사용 방식
- 활성화 프로토콜 　 - **SFTP, FTPS**  
- 자격 증명 공급자 　- **Custom - Amazon API Gateway**  
- 엔드포인트 유형 　 - **VPC, Internet Facing**  
- 도메인 　　　 　　 - **Amazon S3**  
- 사용자 인증 방식　 - **SSH Key Pair, ID/PW**  

<hr>

## 사전 작업
- [IAM, S3 Bucket, SSH Key Pair 생성](https://github.com/kva231/AWS-Tech-Note/blob/master/Migration%20%26%20Transfer/AWS%20Transfer%20Family/%EC%82%AC%EC%A0%84%20%EC%9E%91%EC%97%85.md)
- [서버 및 클라이언트 인증서와 키를 생성 후, ACM 업로드](https://github.com/kva231/AWS-Tech-Note/blob/master/Security%2C%20Identity%2C%20%26%20Compliance/AWS%20Certificate%20Manager/%EC%84%9C%EB%B2%84%20%EB%B0%8F%20%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8%20%EC%9D%B8%EC%A6%9D%EC%84%9C%EC%99%80%20%ED%82%A4%EB%A5%BC%20%EC%83%9D%EC%84%B1%20%ED%9B%84%2C%20ACM%20%EC%97%85%EB%A1%9C%EB%93%9C.md)
- [AWS CLI 설치](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [AWS SAM CLI 설치](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
- VPC, Public Subnet, EIP 생성
- Endpoint에 적용할 VPC default 보안 그룹 설정(SFTP, FTPS Ports)  
  ![Cap 2022-05-08 19-53-29-102](https://user-images.githubusercontent.com/46125158/167293042-5dd3f740-0a0d-4d7c-b1a0-87d909b2482d.png)

<hr>

## 패키지 다운로드 및 실행
1. [링크를 클릭해 패키지 다운로드](https://s3.amazonaws.com/aws-transfer-resources/custom-idp-templates/aws-transfer-custom-idp-secrets-manager-sourceip-protocol-support-apig.zip)

2. 다운로드받은 파일을 압축해제하고 해당 디렉터리로 이동

3. sam 명령 실행
    ```
    aws configure                     # 인증 정보 설정

    sam deploy --guided               # Stack Name: Transfer-Family-using-ID-PW
                                      # AWS Region: ap-northeast-2
                                      # Perameter CreateServer: false
                                      # Parameter SecretManagerRegion: (Enter)
                                      # Parameter TransferEndpointType: (Enter)
                                      # Parameter TransferSubnetIDs: (Enter)
                                      # Parameter TransferVPCID: (Enter)
                                      ...
                                      # Confirm changes before deploy: y
                                      # Allow SAM CLI IAM role creation: y
                                      # Disable rollback: n
                                      # Save arguments to configuration file: y
                                      # SAM configuration file: (Enter)
                                      # SAM configuration environment: (Enter)
                                      ...
                                      # Deploy this changeset?: y
    ```
서버는 직접 생성할 것이므로 EndpointType이나 VPC, Subnet 등의 설정은 하지 않음  
(자동 생성하면 엔트포인트가 없고 EIP를 지정할 수도 없으므로 수동 생성으로 진행)

4\. 패키지로 인해 생성된 대표 리소스 확인
- **CloudFormation**  
  ![Cap 2022-05-08 21-36-21-808](https://user-images.githubusercontent.com/46125158/167296492-db108329-79d4-4403-9178-1296e25ae50e.png)
- **IAM**  
  ![image](https://user-images.githubusercontent.com/46125158/167287338-e7077c90-dd12-42f8-a0e9-83f2686204eb.png)
  ![image](https://user-images.githubusercontent.com/46125158/167287322-3b76ec12-8971-49b7-a3bc-b4bf70180cbf.png)
- **API Gateway**  
  ![image](https://user-images.githubusercontent.com/46125158/167287393-87824ffc-b1d6-4bc1-8bd0-b484c7aaa039.png)
  ![image](https://user-images.githubusercontent.com/46125158/167287479-a9ef0307-57f0-4918-b7cd-7288eaf2f8cc.png)
  ![image1](https://user-images.githubusercontent.com/46125158/167287561-3b45f8cd-c596-407e-8b30-43745e4b5f43.png)
- **Lambda**  
  ![image](https://user-images.githubusercontent.com/46125158/167287594-dc437676-e4ab-45fb-81bf-d1d11fc436bf.png)
  ![image](https://user-images.githubusercontent.com/46125158/167287618-b590575d-9cb6-45cc-8e10-edc4a4a149ec.png)
  ![Cap 2022-05-08 21-43-41-147](https://user-images.githubusercontent.com/46125158/167296721-4bdf23bd-c8fa-453f-9c38-875ff807fd34.png)

<hr>

## 서버 생성
![Cap 2022-05-08 17-24-09-235](https://user-images.githubusercontent.com/46125158/167289083-080b267d-37ed-480f-ba5c-f64bebd275dd.png)
![Cap 2022-05-08 17-25-06-798](https://user-images.githubusercontent.com/46125158/167289084-4a862f12-1a8d-43d7-89f2-7c0673ffbd38.png)
![Cap 2022-05-08 17-30-48-048](https://user-images.githubusercontent.com/46125158/167289085-b18a11fb-77fb-4941-a5a3-304b88863018.png)
![Cap 2022-05-08 17-31-57-355](https://user-images.githubusercontent.com/46125158/167293453-bcc14a0c-a0e9-4584-a6bd-5107d4df0e36.png)
![Cap 2022-05-08 17-32-17-806](https://user-images.githubusercontent.com/46125158/167289088-d524c483-d1c8-44a2-93ee-6e05c8504fea.png)
![Cap 2022-05-08 17-32-32-011](https://user-images.githubusercontent.com/46125158/167289089-d7618328-e006-4a03-8046-e0644e541f0e.png)
![Cap 2022-05-08 17-32-50-408](https://user-images.githubusercontent.com/46125158/167289091-17e43cb8-f214-44d1-876a-eff1daa96f43.png)
![Cap 2022-05-08 17-41-16-738](https://user-images.githubusercontent.com/46125158/167289092-1ae84f5a-50ee-4ffa-bc45-4accc06543a2.png)

<hr>

## AWS Secrets Manager을 이용한 사용자 추가
![Cap 2022-05-08 18-03-36-070](https://user-images.githubusercontent.com/46125158/167289831-512f3c7d-5b17-4ca0-bfd3-051e4b15109e.png)
![Cap 2022-05-08 19-37-50-442](https://user-images.githubusercontent.com/46125158/167292487-6a17d30c-2544-405a-8b71-4253a79b4ee0.png)
![Cap 2022-05-08 18-09-17-433](https://user-images.githubusercontent.com/46125158/167289867-958de0f0-943f-4d40-9b78-dc0f10b70b30.png)
![Cap 2022-05-08 18-09-47-305](https://user-images.githubusercontent.com/46125158/167289909-9ab15913-b47d-4d38-9bb8-0bbe39e3ad4f.png)
![Cap 2022-05-08 18-10-57-521](https://user-images.githubusercontent.com/46125158/167290021-0a6475c8-63c4-4556-9f3a-16fcaef4cd1b.png)
![Cap 2022-05-08 18-11-21-586](https://user-images.githubusercontent.com/46125158/167290123-3acfdc94-fb60-4a15-8058-afaad6ea0914.png)
![Cap 2022-05-08 19-42-37-953](https://user-images.githubusercontent.com/46125158/167292494-030dc29a-a562-4734-be6b-ecbe0e6f936a.png)

<hr>

## FileZilla를 사용하여 파일 전송
- **SSH Key Pair를 이용한 접근**(FTPS는 이용 불가. FTPS는 ID/PW로만 접근 가능)  
  ![Cap 2022-05-08 18-34-19-150](https://user-images.githubusercontent.com/46125158/167290419-e45e544d-3f54-4df0-9fbd-26e493194592.png)
  ![Cap 2022-05-08 18-45-41-181](https://user-images.githubusercontent.com/46125158/167290832-1f8d1df3-0f17-49fc-b04a-fd8c4e593258.png)

- **ID/PW를 이용한 접근**  
  ![Cap 2022-05-08 18-34-19-150](https://user-images.githubusercontent.com/46125158/167290419-e45e544d-3f54-4df0-9fbd-26e493194592.png)
  ![Cap 2022-05-08 18-34-53-074](https://user-images.githubusercontent.com/46125158/167290599-319d7693-30db-4867-8078-c0ad68a0b396.png)
  ![Cap 2022-05-08 19-18-28-431](https://user-images.githubusercontent.com/46125158/167291881-000326dd-9cd8-4c18-8661-05bf0e9dc8dd.png)

- **결과**  
  ![Cap 2022-05-08 19-47-48-601](https://user-images.githubusercontent.com/46125158/167292659-de9a3973-6255-4f3f-aae0-ffed6e878d54.png)

<hr>

## 참고
- **AWS CLI 설치** - https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
- **AWS SAM CLI 설치** - https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html
- **Transfer Family PW 인증 활성화** - https://aws.amazon.com/ko/blogs/storage/enable-password-authentication-for-aws-transfer-family-using-aws-secrets-manager-updated/
- **FTPS 지원 서버 생성** - https://docs.aws.amazon.com/transfer/latest/userguide/create-server-ftps.html
- **Amazon API Gateway 사용하여 자격 증명 공급자 통합** - https://docs.aws.amazon.com/transfer/latest/userguide/custom-identity-provider-users.html#authentication-api-gateway
- **FileZilla 사용** - https://docs.aws.amazon.com/transfer/latest/userguide/transfer-file.html#filezilla
