# 서버 및 클라이언트 인증서와 키를 생성 후, ACM 업로드

<br/>

1. EC2 인스턴스 준비 및 접근
2. git 설치
    ```bash
    sudo yum install -y git
    ```
3. OpenVPN easy-rsa 리포지토리 복제 후, easy-rsa/easyrsa3 폴더로 이동
    ```bash
    git clone https://github.com/OpenVPN/easy-rsa.git
    cd easy-rsa/easyrsa3
    ```
4. 새 PKI 환경 시작
    ```bash
    ./easyrsa init-pki
    ```
5. 새 CA(인증 기관) 빌드
    ```bash
    ./easyrsa build-ca nopass                     # 인증 기관 이름 입력
    ```
6. 서버 인증서 및 키 생성
    ```bash
    ./easyrsa build-server-full server nopass
    ```
7. 클라이언트 인증서 및 키 생성
    ```bash
    ./easyrsa build-client-full client1.domain.tld nopass
    ```
8. 사용자 지정 폴더 생성 후, 해당 폴더에 인증서 및 키를 복사하고 해당 폴더로 이동
    ```bash
    mkdir acm
    cp pki/ca.crt ./acm/
    cp pki/issued/server.crt ./acm/
    cp pki/private/server.key ./acm/
    cp pki/issued/client1.domain.tld.crt ./acm/
    cp pki/private/client1.domain.tld.key ./acm/
    cd ./acm/
    ```
9. 인증서와 키를 ACM에 업로드
    ```bash
    aws configure                               # 인증 정보 설정
    
    aws acm import-certificate --certificate fileb://server.crt --private-key fileb://server.key --certificate-chain fileb://ca.crt
    aws acm import-certificate --certificate fileb://client1.domain.tld.crt --private-key fileb://client1.domain.tld.key --certificate-chain fileb://ca.crt
    ```
10. ACM에서 업로드된 인증서 확인
    ![Cap 2022-05-06 22-11-33-830](https://user-images.githubusercontent.com/46125158/167138223-dc66cba8-1e40-472e-aec9-963ac72bc770.png)

