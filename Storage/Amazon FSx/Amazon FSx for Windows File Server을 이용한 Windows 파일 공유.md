# Amazon FSx for Windows File Server을 이용한 Windows 파일 공유

<br/>

## Architecture 참고
![image](https://user-images.githubusercontent.com/46125158/168588704-45445fb5-8e06-477e-80f2-3a263b721d09.png)

<hr>

## 사전 작업
- Public Subnet, Private Subnet 각각 2개씩 생성
- 각 Public Subnet에 EC2 Windows 인스턴스 생성
- [AWS Managed Microsoft Active Directory(AD) 생성](https://github.com/kva231/AWS-Tech-Note/blob/master/Security%2C%20Identity%2C%20%26%20Compliance/AWS%20Directory%20Service/AWS%20Managed%20Microsoft%20Active%20Directory(AD)%20%EC%83%9D%EC%84%B1.md)

<hr>

## EC2 인스턴스 접속
- **보안 그룹 인바운드 설정**  


<hr>

## Domain Join


<hr>

## Amazon FSx for Windows File Server 생성
- **보안 그룹 생성**  


<hr>

## Network Drive 매핑

<hr>

## 파일 공유 테스트 및 확인

<hr>

## 참고
- **보안 그룹 설정** - https://docs.aws.amazon.com/fsx/latest/WindowsGuide/limit-access-security-groups.html
- **Windows 인스턴스 수동 도메인 조인** - https://docs.aws.amazon.com/directoryservice/latest/admin-guide/join_windows_instance.html
