# WAF logs를 S3로 전송

<br/>

## Architecture 참고
![Image](https://user-images.githubusercontent.com/46125158/175768928-32fa8a3c-dae6-4e04-88fd-325a5129483e.png)

<hr>

## 사전 작업
**※ WAF, Kinesis Data Firehose, S3 모두 동일 리전에 생성해야 함**

### ALB, EC2 생성 후 연결
![Cap 2022-06-25 17-14-32-864](https://user-images.githubusercontent.com/46125158/175768956-7e2c968f-ea5a-4bc3-974e-611681aa8c45.png)   
![Cap 2022-06-25 17-14-57-750](https://user-images.githubusercontent.com/46125158/175768962-37c9f57e-487f-4b66-967c-1d29dbc5563f.png)

### S3 생성
![Cap 2022-06-25 17-15-35-495](https://user-images.githubusercontent.com/46125158/175769001-4448fa6f-f373-45bd-b888-b91d11dfb013.png)

### WAF 생성(ALB 연결)
![Cap 2022-06-25 17-21-11-711](https://user-images.githubusercontent.com/46125158/175769006-40b7a1d8-ee4e-4e9e-a170-3f7f5005b23d.png)  
![Cap 2022-06-25 17-21-29-356](https://user-images.githubusercontent.com/46125158/175769080-ba269364-3310-4a0f-a56d-e1250955cac0.png)  
![Cap 2022-06-25 17-21-44-363](https://user-images.githubusercontent.com/46125158/175769102-d685bc92-7bbd-4a40-885b-d18114c9dd6a.png)

<hr>

## Amazon Kinesis Data Firehose 생성
![Cap 2022-06-25 17-24-05-656](https://user-images.githubusercontent.com/46125158/175769853-9156eb60-b6fb-4a7b-be12-b92886b013af.png)

### Source, Destination, Delivery stream name 설정
![Cap 2022-06-25 17-57-54-718](https://user-images.githubusercontent.com/46125158/175769900-ec071c85-a44a-4d60-b1a3-3b8183783750.png)  
- Source  
  데이터 생성자. 레코드를 Kinesis Data Firehose 전송 스트림으로 전달. 여기선 AWS WAF Web ACL logs
- Destination  
  Kinesis Data Firehose 전송 스트림의 대상. 여기선 Amazon S3
- Delivery stream name  
  WAF에서 Logging 설정하려면 반드시 'aws-waf-logs-'로 시작해야 함

### Destination 설정
![Cap 2022-06-25 17-58-20-035](https://user-images.githubusercontent.com/46125158/175770161-b969f780-425c-4acd-aaaa-c5b4b4673340.png)  
![Cap 2022-06-25 17-58-54-573](https://user-images.githubusercontent.com/46125158/175770350-68f2266e-42e0-420b-aa4b-acf7bfa8bf88.png)  
- Buffer size 및 Buffer interval  
  Kinesis Data Firehose는 수신 스트리밍 데이터를 특정 크기로 또는 특정 기간 동안 버퍼링하여 대상으로 전달

### Advanced 설정
![Cap 2022-06-25 17-59-34-174](https://user-images.githubusercontent.com/46125158/175770529-2f43944a-cf0f-488c-8567-d22cbe4724bb.png)  
- Amazon CloudWatch error logging  
  Kinesis Data Firehose가 CloudWatch Logs에 레코드 전송 오류를 기록하도록 하려면 Enabled 선택

### 생성된 리소스 확인
#### Delivery stream
![Cap 2022-06-25 18-00-39-042](https://user-images.githubusercontent.com/46125158/175770905-28813cd7-adc3-4c64-8f09-d6f5e181ac31.png)

#### CloudWatch Log group
![Cap 2022-06-25 18-01-18-289](https://user-images.githubusercontent.com/46125158/175770962-10063ef9-f628-4f22-bb78-56847cfb649a.png)  

#### IAM Role
![Cap 2022-06-25 18-43-58-355](https://user-images.githubusercontent.com/46125158/175771034-a8382b34-e753-42a4-93bc-3cb062de7f2b.png)

<hr>

## 샘플 데이터를 사용하여 전송 스트림 테스트
### Deliverty Stream 선택 후 테스트 진행
![Cap 2022-06-25 18-50-46-402](https://user-images.githubusercontent.com/46125158/175772759-9984f62f-14d2-45ba-855e-e56d7f2766ef.png)

### S3 Bucket에 데이터가 전송되었는지 확인
![Cap 2022-06-25 18-56-50-381](https://user-images.githubusercontent.com/46125158/175772724-a9833f37-136c-46e8-83b2-b71b7ad5812d.png)  
![Cap 2022-06-25 18-58-00-714](https://user-images.githubusercontent.com/46125158/175772739-9ca82df5-dcfc-4e01-b315-b5e7bb4409ab.png)

### ※ CloudWatch Log group error logs 전송 테스트
#### Amazon Kinesis Data Firehose 생성하면서 만들어진 IAM role(KinesisFirehoseServiceRole-aws-waf~...)에서 S3 관련 권한 제거 후, 테스트 진행
![Cap 2022-06-25 22-14-00-298](https://user-images.githubusercontent.com/46125158/175775211-53c8c840-61d5-44c0-b364-062417956785.png)

<hr>

## AWS WAF를 Kinesis Data Firehose와 연결
### WAF Web ACL 선택
![Cap 2022-06-25 18-46-20-267](https://user-images.githubusercontent.com/46125158/175772262-6d3ce07c-96ef-452e-9097-1d5cb92949b7.png)

### 로깅 활성화
![Cap 2022-06-25 18-46-40-730](https://user-images.githubusercontent.com/46125158/175774108-a4da0b28-0e67-4fc4-913c-9f422c62afaf.png)  
- IAM role  
  AWS WAF에서 이 역할을 관리. AWS WAF에 'aws-waf-logs-'로 시작하는 Kinesis Data Firehose 전송 스트림 쓰기 권한 부여

### 확인
#### 로깅 활성화 상태
![Cap 2022-06-25 20-46-36-990](https://user-images.githubusercontent.com/46125158/175772446-b796b331-3b08-4988-b40f-687127761808.png)

#### IAM Role
![Cap 2022-06-25 22-03-36-831](https://user-images.githubusercontent.com/46125158/175774681-14443db5-8845-450a-8932-1a18fa17cba3.png)

<hr>

## 참고
- **AWS Support** - https://aws.amazon.com/ko/premiumsupport/knowledge-center/waf-configure-comprehensive-logging/
- **Amazon Kinesis Data Firehose 전송 스트림 생성** - https://docs.aws.amazon.com/firehose/latest/dev/basic-create.html
- **샘플 데이터를 사용하여 전송 스트림 테스트** - https://docs.aws.amazon.com/firehose/latest/dev/test-drive-firehose.html
- **CloudWatch Logs를 사용하여 Kinesis Data Firehose 모니터링** - https://docs.aws.amazon.com/firehose/latest/dev/monitoring-with-cloudwatch-logs.html
