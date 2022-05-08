# SFTP, FTPS 지원 서버 생성 및 파일 전송

## 사용 방식
- 활성화 프로토콜 　 - **SFTP, FTPS**  
- 자격 증명 공급자 　- **Custom - Amazon API Gateway**  
- 엔드포인트 유형 　 - **VPC, Internet Facing**  
- 도메인 　　　 　　 - **Amazon S3**  
- 사용자 인증 방식　 - **SSH Key Pair, ID/PW**  

<hr>

## 사전 작업
- [IAM, S3 Bucket, SSH Key Pair 생성](https://github.com/kva231/AWS-Tech-Note/blob/master/Migration%20%26%20Transfer/AWS%20Transfer%20Family/%EC%82%AC%EC%A0%84%20%EC%9E%91%EC%97%85.md)
- [AWS CLI 설치](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [AWS SAM CLI 설치](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
- VPC, Puvlic Subnet, EIP 준비

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

<hr>

## 서버 생성

<hr>

## Amazon S3 서비스 관리 사용자 추가

<hr>

## FileZilla를 사용하여 파일 전송

<hr>

## 참고
- **Transfer Family PW 인증 활성화** - https://aws.amazon.com/ko/blogs/storage/enable-password-authentication-for-aws-transfer-family-using-aws-secrets-manager-updated/
