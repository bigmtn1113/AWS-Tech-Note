# AWS CLI를 이용한 Assume Role 사용 - 다른 계정 리소스에 접근하기

<br>

## 시나리오
Dev 계정의 EC2에서 Prod 계정의 S3 버킷 리스트를 조회해야 하는 상황

<br>

## Architecture
![Image](https://user-images.githubusercontent.com/46125158/193773993-126ad4f3-33f5-4a8f-b99e-14ffc0373381.png)

<br>

## 사전 작업

- **AWS 계정 두 개(Dev. Prod) 준비**
- **Dev 계정에 EC2 생성**
- **Dev 계정의 EC2에 jq 설치** - 문자열 추출 목적
  - ```bash
    sudo yum -y install jq
    ```
- **Dev 계정에 아무 권한이 없는 IAM Role 생성 후, EC2에 연결**
  - Role 이름 - EmptyRole
- **Prod 계정에 테스트 용 S3 버킷 준비**

<hr/>

## IAM Role 생성 및 편집
Prod 계정에 S3 권한이 있는 IAM Role 생성  
DEV 계정의 EC2가 권한을 빌려서 사용할 IAM Role

### 신뢰할 수 있는 엔터티 선택
![Cap 2022-10-02 16-06-19-196](https://user-images.githubusercontent.com/46125158/193443990-a2072624-807b-403c-a601-dff9eb02a0dc.png)

### 권한 추가
![Cap 2022-10-02 16-07-39-375](https://user-images.githubusercontent.com/46125158/193443999-977553cf-f6ae-4fdb-934b-103017b6e695.png)

### 이름 지정, 검토 및 생성
![Cap 2022-10-02 16-09-40-867](https://user-images.githubusercontent.com/46125158/193444003-64ea3284-2b6e-4215-93d3-9c8737bd702a.png)

### 생성한 Role 선택 후, 신뢰 정책 편집
![Cap 2022-10-02 16-11-08-293](https://user-images.githubusercontent.com/46125158/193444064-ab8c6f40-e9e1-477e-8b27-30a5aad48ce7.png)  
![Cap 2022-10-02 16-11-52-092](https://user-images.githubusercontent.com/46125158/193444132-73dc0b1e-ec1c-4381-b0ab-0355453cc430.png)  
- Dev 계정의 EC2에 연결된 IAM Role(EmptyRole)을 신뢰하는 정책 추가

<br>

## IAM Policy 생성 및 연결
Dev 계정의 EC2에 연결된 IAM Role(EmptyRole)에 Prod 계정에서 만든 IAM Role(CrossAccountRole-EC2ToS3) 권한 부여

### 인라인 정책 생성 및 확인
정책 생성 후 연결해도 무방하나, 편의상 인라인 정책으로 진행

![Cap 2022-10-02 16-25-10-663](https://user-images.githubusercontent.com/46125158/193444327-b4a069e0-8d1a-4ada-87e8-3943eb318844.png)  
![Cap 2022-10-02 16-25-47-035](https://user-images.githubusercontent.com/46125158/193444350-ba75fb98-a33b-445c-8400-49db0ba3de66.png)  
![Cap 2022-10-02 16-29-03-697](https://user-images.githubusercontent.com/46125158/193444365-37372970-da71-45ce-b661-7898b39b8938.png)  
![Cap 2022-10-02 16-30-03-584](https://user-images.githubusercontent.com/46125158/193444384-9dc83e5b-df30-486a-bdea-8147985a48fc.png)  

<br>

## 임시 토큰 발급 및 Profile 등록
Dev 계정의 EC2에 접근 후 작업 진행

### aws sts assume-role
```bash
# <변수 명>=$(aws sts assume-role --role-arn "arn:aws:iam::<Prod 계정 ID>:role/CrossAccountRole-EC2ToS3" --role-session-name <세션 이름>)

OUT=$(aws sts assume-role --role-arn "arn:aws:iam::123456789012:role/example-role" --role-session-name AWSCLI-Session)
```

### 변수에서 AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_SESSION_TOKEN 추출
```bash
# AWS_ACCESS_KEY_ID=$(echo $<변수 명> | jq -r '.Credentials''.AccessKeyId')
# AWS_SECRET_ACCESS_KEY=$(echo $<변수 명> | jq -r '.Credentials''.SecretAccessKey')
# AWS_SESSION_TOKEN=$(echo $<변수 명> | jq -r '.Credentials''.SessionToken')

AWS_ACCESS_KEY_ID=$(echo $OUT | jq -r '.Credentials''.AccessKeyId')
AWS_SECRET_ACCESS_KEY=$(echo $OUT | jq -r '.Credentials''.SecretAccessKey')
AWS_SESSION_TOKEN=$(echo $OUT | jq -r '.Credentials''.SessionToken')
```

### Profile 설정
~/.aws/credentials에 Profile 등록  
여기선 Profile 이름을 Demo라고 지정

```bash
# aws configure set aws_access_key_id "$AWS_ACCESS_KEY_ID" --profile <Profile 이름>
# aws configure set aws_secret_access_key "$AWS_SECRET_ACCESS_KEY" --profile <Profile 이름>
# aws configure set aws_session_token "$AWS_SESSION_TOKEN" --profile <Profile 이름>

aws configure set aws_access_key_id "$AWS_ACCESS_KEY_ID" --profile Demo
aws configure set aws_secret_access_key "$AWS_SECRET_ACCESS_KEY" --profile Demo
aws configure set aws_session_token "$AWS_SESSION_TOKEN" --profile Demo
```

### 등록된 Profile 확인
```bash
# Profile 이름, aws_access_key_id, aws_secret_access_key, aws_session_token 확인

cat ~/.aws/credentials
```

### Account, UserId, Arn 확인
```bash
# aws sts get-caller-identity --profile <Profile 이름>

aws sts get-caller-identity --profile Demo
```

<br>

## S3 버킷 리스트 조회
Dev 계정의 EC2에서 Prod 계정의 S3 버킷 리스트 조회

### 등록한 Profile을 이용해 S3 버킷 리스트 조회
![Cap 2022-10-02 16-49-25-615](https://user-images.githubusercontent.com/46125158/193444421-0ec72524-6fe3-4686-a668-d532411d25d1.png)

<hr>

## 참고
- **AWS CLI를 사용하여 IAM 역할 위임** - https://aws.amazon.com/ko/premiumsupport/knowledge-center/iam-assume-role-cli/
