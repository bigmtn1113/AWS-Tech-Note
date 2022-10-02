# AWS CLI를 이용한 Assume Role 사용(다른 계정 리소스에 접근하기)

<br>

## 시나리오
Dev 계정의 EC2에서 Prod 계정의 S3 버킷 리스트를 조회해야 하는 상황

## Architecture


<br>

## 사전 작업

- **AWS 계정 두 개(Dev. Prod) 준비**
- **Dev 계정에 EC2 생성**
- **Dev 계정의 EC2에 jq 설치**
  - ```bash
    sudo yum -y install jq
    ```
- **Dev 계정에 아무 권한이 없는 IAM Role 생성 후, EC2에 연결**
  - Role 이름 - EmptyRole
- **Prod 계정에 테스트 용 S3 버킷 준비**

<hr/>

## Prod 계정에 S3 권한이 있는 IAM Role 생성
DEV 계정의 EC2가 사용할 IAM Role 생성

### 신뢰할 수 있는 엔터티 선택


### 권한 추가


### 이름 지정, 검토 및 생성


### 생성한 Role 선택 후, 정책 편집


<br>

## Dev 계정의 EC2에 연결된 IAM Role에 Prod 계정에서 만든 IAM Role 권한 부여
### 인라인 정책 생성 및 확인
정책 생성 후 연결해도 무방하나, 편의상 인라인 정책으로 진행


<br>

## Dev 계정의 EC2에 접근 후, 임시 토큰을 발급받고 Profile에 등록
### aws sts assume-role 명령어 사용 후, 출력 값 변수에 저장
```bash
# ex) OUT=$(aws sts assume-role --role-arn "arn:aws:iam::123456789012:role/example-role" --role-session-name AWSCLI-Session)

<변수 명>=$(aws sts assume-role --role-arn "arn:aws:iam::<Prod 계정 ID>:role/CrossAccountRole-EC2ToS3" --role-session-name <세션 이름>)
```

### 변수에서 AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_SESSION_TOKEN 추출
```bash
AWS_ACCESS_KEY_ID=$(echo $<변수 명> | jq -r '.Credentials''.AccessKeyId')
AWS_SECRET_ACCESS_KEY=$(echo $<변수 명> | jq -r '.Credentials''.SecretAccessKey')
AWS_SESSION_TOKEN=$(echo $<변수 명> | jq -r '.Credentials''.SessionToken')
```

### Profile 설정
~/.aws/credentials에 Profile 등록  
여기선 Profile 이름을 Demo라고 지정

```bash
aws configure set aws_access_key_id "$AWS_ACCESS_KEY_ID" --profile <Profile 이름>
aws configure set aws_secret_access_key "$AWS_SECRET_ACCESS_KEY" --profile <Profile 이름>
aws configure set aws_session_token "$AWS_SESSION_TOKEN" --profile <Profile 이름>
```

### Account, UserId, Arn 확인
```bash
aws sts get-caller-identity --profile <Profile 이름>
```

<br>

## Dev 계정의 EC2에서 Prod 계정의 S3 버킷 리스트 조회


<hr>

## 참고
- **참고** - 링크
