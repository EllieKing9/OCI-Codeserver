# OCI Code_server Nginx

Oracle Cloud Free Tier 가입 https://www.oracle.com/kr/cloud/

컴퓨트 인스턴스 생성

1. 리소스실행 > VM 인스턴스 생성 > VM.Standard.E2.1.Micro (Free)

  - 가상 머신, 1 core OCPU(Oracle ComPute Unit), 1 GB memory, 0.48 Gbps network bandwidth
  
  - https://docs.oracle.com/en-us/iaas/Content/Compute/References/computeshapes.htm
  
  - OCI 기본용어 http://taewan.kim/oci_docs/00_oci/terminologies/

2. 이미지 및 구성 > 편집 > 이미지 변경

3. 네트워킹 > 편집
  - 공용 IPv4 주소 지정(공용 IP 주소를 지정하면 인터넷에서 이 인스턴스에 액세스할 수 있게 됩니다. 공용 IP 주소가 필요한지 여부를 모를 경우 항상 나중에 지정할 수 있습니다.)

4. SSH 키 추가 > 자동으로 키 쌍 생성 > 전용(*.key), 공용(*.key.pub) 키 저장

5. 인스턴스 생성 완료 후
```
햄버거 _ 컴퓨트 _ 인스턴스 _ 해당 인스턴스(인스턴스 세부정보) _ 
리소스(왼쪽 아래) _ 연결된 VNIC _ VNIC 네임 _ 
리소스(왼쪽 아래) _ IPv4 주소 _ 편집에서 
공용(인스턴스 종료_삭제시 만료되어 사용불가?) IPv4 주소를 예약된 주소로 변경가능
```
  - https://docs.oracle.com/en-us/iaas/Content/Network/Tasks/managingpublicIPs.htm

-------------------------
SSH 키 쌍을 자동으로 생성하여 저장한 경우

PuTTYgen > Load > all files > *.key 선택 > Successfully imported > Save private key
*.ppk 로 저장하여 PuTTY SSH 접속시 사용

--------------------------
PuTTY 세팅

host Name: OCI VM IP address 
Connection type: SSH
Putty > Category > Connection > SSH > Auth > Browse... > *.ppk 선택
Putty > Category > Connection > Data > Auto-login username 기입

--------------------------

SSH 접속
pwd > /homw/ubuntu

code server 설치
```
//자동설치
$sudo curl -fsSL https://code-server.dev/install.sh | sh

//수동설치
$mkdir -p ~/.cache/code-server
$curl -#fL -o ~/.cache/code-server/code-server_4.7.0_amd64.deb.incomplete -C -https://github.com/coder/code-server/releases/download/v4.7.0/code-server_4.7.0_amd64.deb
$mv ~/.cache/code-server/code-server_4.7.0_amd64.deb.incomplete ~/.cache/code-server/code-server_4.7.0_amd64.deb
$sudo dpkg -i ~/.cache/code-server/code-server_4.7.0_amd64.deb

//설정
$sudo nano ~/.config/code-server/config.yaml

//실행
$code-server
  + info Wrote default config file to ~/.config/code-server/config.yaml
```
설정 참고: https://github.com/EllieKing9/node

error RSA PRIVATE KEY not found from openssl output 인 경우
*pem 를 직접 등록
1. mkcert로 유효한 키를 수동으로 만들어서 사용
mkcert 설명 : https://github.com/coder/code-server/issues/5162
```
$nano ~/.config/code-server/config.yaml
  + cert: 경로/*.pem
  + cert-key: 경로/*key.pem
```
2. openssl로 인증서 만들기?!

Ubuntu 실행시 code-server가 자동 실행 되도록 service 등록?!
```
$sudo systemctl enable --now code-server
//sudo systemctl enable --now code-server@$USER
! WSL에서는 systemctl 를 지원하지 않는다 //systemctl(x) -> service(o)
$sudo systemctl start code-server //실행
$sudo systemctl stop code-server //중지
$sudo systemctl restart code-server //재시작
$sudo systemctl status code-server 
```
------------------------------

방화벽 세팅하여 사용
  - https://blog.naver.com/afy/222720018657
 
  - https://youngq.tistory.com/97?category=868706
 
  - OCI _ Home _ Networking _ VCN _ VCN name 클릭 _ Subnet name 클릭 _ Security List name 클릭 _ Inbound 규칙 추가 _ 0.0.0.0/0 TCP "8080"
  
  - ubuntu 접속 _ 세팅 부랴부랴
  $sudo iptables -I INPUT 1 -p tcp --dport 80 -j ACCEPT
  
Nginx 설치해서 사용
  - https://hakawati.co.kr/445
  
  - Nginx 설명 https://icarus8050.tistory.com/57    |   https://extrememanual.net/9976
  - 환경 설정 https://12bme.tistory.com/366   |   https://server-talk.tistory.com/304
  - SSL 설정 https://server-talk.tistory.com/315?category=925489
  - 가상 호스트 https://www.lesstif.com/system-admin/nginx-24444975.html

```

```

