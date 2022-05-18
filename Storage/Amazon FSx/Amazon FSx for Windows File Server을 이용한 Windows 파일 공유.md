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
- **EC2 보안 그룹 인바운드 설정**  
  ![Cap 2022-05-17 21-06-56-320](https://user-images.githubusercontent.com/46125158/168967439-581647f4-5be7-4848-912e-325b63ba4b02.png)

- **EC2 Password 확인**  
  ![Cap 2022-05-17 21-02-59-681](https://user-images.githubusercontent.com/46125158/168967604-652b89d2-5905-49f1-bbd4-1985f81b6b89.png)  
  ![Cap 2022-05-17 21-03-18-231](https://user-images.githubusercontent.com/46125158/168967613-3df947a7-4d7c-4940-81c0-a131408a27b3.png)  
  ![Cap 2022-05-17 21-03-31-064](https://user-images.githubusercontent.com/46125158/168967625-cf8aa238-b504-4263-9bae-317a7e16823f.png)

- **로그인**  
  ![Cap 2022-05-17 21-07-57-286](https://user-images.githubusercontent.com/46125158/168968967-69a6fe77-cdc3-4299-a2d6-d117cf49fadf.png)  

<hr>

## Domain Join
- **AWS Directory Service 제공 DNS 서버 IP 확인**  
  ![Cap 2022-05-17 21-10-59-956](https://user-images.githubusercontent.com/46125158/168971280-b4fcb238-3e38-4474-943f-82c483a34e52.png)

- **AWS Directory Service 제공 DNS 서버 사용**  
  ![Cap 2022-05-17 21-09-46-712](https://user-images.githubusercontent.com/46125158/168970469-5e0a5eb5-0d34-4383-ab8e-37f481ef2a40.png)  
  ![Cap 2022-05-17 21-09-57-307](https://user-images.githubusercontent.com/46125158/168970541-03b6da53-e9d8-451b-bdfc-4ca6a2bfaa87.png)  
  ![Cap 2022-05-17 21-10-05-159](https://user-images.githubusercontent.com/46125158/168970583-4bf7cf7e-0dd2-418f-a973-ce65ae7db1b8.png)  
  ![Cap 2022-05-17 21-10-11-624](https://user-images.githubusercontent.com/46125158/168970667-4fffcb2a-6937-4770-a64b-2bd1c1424c6a.png)  
  ![Cap 2022-05-17 21-11-51-804](https://user-images.githubusercontent.com/46125158/168971111-41a5c521-4dcc-41bd-b2e6-4f4b06b96b53.png)  

- **Domain 설정**  
  ![Cap 2022-05-17 21-12-42-469](https://user-images.githubusercontent.com/46125158/168972849-f4f57f34-996b-416c-b829-f021c69bc716.png)  
  ![Cap 2022-05-17 21-12-54-315](https://user-images.githubusercontent.com/46125158/168972940-3db8dee7-6184-48b5-a41c-5547b0358829.png)  
  ![Cap 2022-05-17 21-13-07-452](https://user-images.githubusercontent.com/46125158/168973081-5230cd48-c74b-4136-94bb-c40489d44928.png)  
  ![Cap 2022-05-17 21-13-30-898](https://user-images.githubusercontent.com/46125158/168973160-bbaff87e-8b38-4228-ae3f-52aa1a44cfdc.png)  
  ![Cap 2022-05-17 21-13-41-215](https://user-images.githubusercontent.com/46125158/168973912-5240d9bf-f46a-4bb6-ae58-babb02257de8.png)  
  ※ **사용자 이름 및 암호 입력**  
  도메인의 정규화된 이름이나 NetBios 이름, 백슬래시(\\), 사용자 이름 차례로 입력 가능  
  AWS Managed Microsoft AD를 사용하는 경우 사용자 이름은 Admin  
  ex) bigmtn.com\admin 또는 bigmtn\admin  
  ![Cap 2022-05-17 21-14-10-085](https://user-images.githubusercontent.com/46125158/168986537-fec2bc72-f7c8-4277-a2db-588bc5e54d9d.png)  
  **재부팅**  
  ![Cap 2022-05-17 21-14-46-340](https://user-images.githubusercontent.com/46125158/168989290-c873e54e-d45d-4edb-b7c7-3363b11d5903.png)

<hr>

## Amazon FSx for Windows File Server 생성
- **FSx에 적용할 보안 그룹 생성**  
  ![Cap 2022-05-17 21-19-21-178](https://user-images.githubusercontent.com/46125158/169027314-973a2535-6074-4a7a-95c3-6c503860e444.png)  
  ![Cap 2022-05-17 21-24-21-639](https://user-images.githubusercontent.com/46125158/169027646-5daec901-7a99-48da-953d-63df8b1c4bfd.png)

- **Amazon FSx for Windows File Server 생성**  
  ![Cap 2022-05-17 21-15-22-357](https://user-images.githubusercontent.com/46125158/169027793-7db20461-80e7-424f-9841-35dda07881f5.png)  
  ![Cap 2022-05-17 21-15-41-341](https://user-images.githubusercontent.com/46125158/169027877-414b9ebd-f7ca-42bf-9310-ee6971b540f4.png)  
  ![Cap 2022-05-17 21-16-36-506](https://user-images.githubusercontent.com/46125158/169028165-7976b787-a7ca-4ed7-a118-64bf0fc2d3ae.png)  
  ![Cap 2022-05-17 21-25-48-446](https://user-images.githubusercontent.com/46125158/169028699-c2a83357-704c-4a9f-ab96-9e19d9cd5820.png)  
  ![Cap 2022-05-17 21-26-34-694](https://user-images.githubusercontent.com/46125158/169028793-11ba851c-b2d8-44bf-93f9-26cdbf09acfe.png)  
  ![Cap 2022-05-17 21-51-29-641](https://user-images.githubusercontent.com/46125158/169029028-7bad82e5-8e53-4387-b902-6463377ac7fd.png)

<hr>

## Network Drive 매핑

<hr>

## 파일 공유 테스트 및 확인

<hr>

## 참고
- **보안 그룹 설정** - https://docs.aws.amazon.com/fsx/latest/WindowsGuide/limit-access-security-groups.html
- **RDP를 사용하여 Windows 인스턴스에 연결** - https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/connecting_to_windows_instance.html#connect-rdp
- **Windows 인스턴스 수동 도메인 조인** - https://docs.aws.amazon.com/directoryservice/latest/admin-guide/join_windows_instance.html
- **AWS Managed Microsoft AD에 대한 디렉터리 조인 권한 위임** - https://docs.aws.amazon.com/directoryservice/latest/admin-guide/directory_join_privileges.html
