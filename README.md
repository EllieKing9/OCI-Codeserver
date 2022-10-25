# OCI Code_server Nginx

Oracle Cloud Free Tier 가입

https://www.oracle.com/kr/cloud/

  - https://docs.oracle.com/en-us/iaas/Content/Compute/References/computeshapes.htm
  
  - OCI 기본용어 http://taewan.kim/oci_docs/00_oci/terminologies/

  - OCPU (Oracle ComPute Unit)

리소스실행 > VM 인스턴스 생성 > VM.Standard.E2.1.Micro (Free)

  - 가상 머신, 1 core OCPU, 1 GB memory, 0.48 Gbps network bandwidth

이미지 및 구성 > 편집 > 이미지 변경

공용 IPv4 주소

공용 IP 주소를 지정하면 인터넷에서 이 인스턴스에 액세스할 수 있게 됩니다. 공용 IP 주소가 필요한지 여부를 모를 경우 항상 나중에 지정할 수 있습니다.

인스턴스 생성 완료 후

리소스 _ VINC _ 인스턴스네임 _ 리소스 _ IPv4 _ 편집에서 공용(인스턴스 종료_삭제시 만료되어 사용불가) IPv4 주소를 예약된 주소로 변경가능

https://docs.oracle.com/en-us/iaas/Content/Network/Tasks/managingpublicIPs.htm

SSH 키 추가 > 자동으로 키 쌍 생성 > 전용(*.key), 공용(*.key.pub) 키 저장

-------------------------

SSH 키 쌍을 자동으로 생성하여 저장한 경우

PuTTYgen > Load > all files > *.key 선택 > Successfully imported > Save private key
*.ppk 로 저장됨

--------------------------
PuTTY 세팅

host Name: OCI VM IP address 
Connection type: SSH
Putty > SSH > Auth > Browse... > *.ppk 선택

--------------------------

pwd > /homw/ubuntu

code server 설치

$sudo curl -fsSL https://code-server.dev/install.sh | sh

  + mkdir -p ~/.cache/code-server
  
  + curl -#fL -o ~/.cache/code-server/code-server_4.7.0_amd64.deb.incomplete -C -https://github.com/coder/code-server/releases/download/v4.7.0/code-server_4.7.0_amd64.deb

  + mv ~/.cache/code-server/code-server_4.7.0_amd64.deb.incomplete ~/.cache/code-server/code-server_4.7.0_amd64.deb
  
  + sudo dpkg -i ~/.cache/code-server/code-server_4.7.0_amd64.deb

$code-server  

  - info Wrote default config file to ~/.config/code-server/config.yaml
  - 설정 변경: $sudo nano ~/.config/code-server/config.yaml

Ubuntu 실행시 code-server가 자동 실행 되도록 service 등록!

------------------------------

방화벽 세팅하여 사용
  - https://blog.naver.com/afy/222720018657   |   https://youngq.tistory.com/97?category=868706
  - OCI _ Home _ Networking _ VCN _ VCN name 클릭 _ Subnet name 클릭 _ Security List name 클릭 _ Inbound 규칙 추가 _ 0.0.0.0/0 TCP "8080"
  - ubuntu 접속 _ 세팅 부랴부랴
  
Nginx 설치해서 사용
  - https://hakawati.co.kr/445
  
  - Nginx 설명 https://icarus8050.tistory.com/57    |   https://extrememanual.net/9976
  - 환경 설정 https://12bme.tistory.com/366   |   https://server-talk.tistory.com/304
  - SSL 설정 https://server-talk.tistory.com/315?category=925489
  - 가상 호스트 https://www.lesstif.com/system-admin/nginx-24444975.html



