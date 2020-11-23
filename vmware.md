### 172.16.0.*/24 로 서버 네트워크 설정
onpremise 환경에서는 esxi 와 외부 네트워크 연결 x
실제
### 172.16.1~8.*/24 로 개인 네트워크 설정
11/19일 사용 서버 ip 
172.16.0.1 사용

### 비밀번호
root
VMware1!

### 개인 server 사용
vcpu = 4개 (virtual 선택 해서 가상화의 가상화 라는거 명시해 줘야함)
Ram = 16G
ssd = 
Lan = 3개

-- http 로 접근
새 vm 생성
### cpu 부분
cpu 4개 socket 2개 , 게스트 운영체제에 하드웨어 가상화 지원 체크
### momory 
16384MB 로 작성
### ssd
기본 용량 설정하고, thin provisioning 으로 체크
### 네트워크
새 네트워크 추가 해서 총 네트워크 lan 3개 확인

##### 새로 생성한 가상머신
vmware esxi 6.7 iso 버전 파일 cd/rom 에 접속 시켜서
전원 켜야함
※ 중요한점, 설정편집 -> vm옵션 -> booting 옵션 -> bios 설정 

#### 가상 머신을 킨다음
기본적인 설치 하면됨 (key board type - usdefault  선택)

### 새 스위치를 만듬
이름 - internet switch
uplink - vmnic3-아래로
보안 = 모두 동의 한후 추가

### vswitch 0 번도 보안 모두 동의 

### 한 후에  포트그룹 생성
- internet 포트 그룹 - vswitch0번,
- management 포트 그룹 - internet swicth 

### 각 개인 서버에 1번 lan 은 내부 포트그룹에 추가
### 각 개인 서버에 2번 lan 은 인터넷 포트 그룹에 추가

### 각 서버에 ip 부여

ipv4 에 172.16.0.21~172.16.0.23
mask 255.255.255.0
gateway 172.16.0.254
dns설정 172.16.0.25
255.255.255.0
host이름 설정 - esxi-01~03
dns suffix~~ - 사용할 dns 주소 입력

// suffix 란 무엇일까
### vcenter를 사용하기 위해서 dns 서버가 있어야 하는데
### window dns server 를 사용해서 dns서버 설치 해줌

실제 서버에 window server 설치
ssd 만 thin 으로 설정 , 네트워크 어뎁터 2개로 설정
그러고 걍 설치하면됨 ㅇㅇ
dns server로 사용할 거여서 상관없음


### vcenter 를 사용하려면 pc에 있는 ovf 파일을
### esxi -> vm으로 올려야 하는데 이때 stage1 과 stage2 가 있다.

### window 에서 
ipconfig 로 192로 되어있는 eth 찾기
ip 172.16.0.25
dns = 자기자신으로 하기
ipconfig 로 설정 잘 되었는지 확인

server manager 들어가서 add roles -> dns server 체크
install 하면 설치 끝

## 방화벽 설정 방법
# window server 안에서 controll pennal 에서
방화벽 끄면됨
하고 local pc 에서 ping 치면 ping 감

본 서버 로 와서 window-server 오른쪽마우스
guest 운영체제 -> vm ware tools 설치

window 서버에서 server manager -> tools-> dns
들어가서 왼쪽 본인 서버에서 오른쪽마우스 -> new zone

primary zone -> forward lookup zone -> 
zone name 에 본인 dns 주소 넣어주면됨 (ex) jaehotest.net 넣어주고 next 해서 만들어주면 됨

오른쪽에 forward lookup zones 파일 아래에 dns이름으로
폴더가 생성되어 있는데 그거들어가서
new host 한 후에 
esxi 1~3번 까지 host name 과 ip넣어주면 됨
new host 하고
vcenter 이름 넣고, ip = 172.16.0.26(6번으로)

## local pc에서
cmd 에서 nslookup 한후
server 172.16.0.25 기입
한 후에 hostname,dnsname 쳐서 ip s나오는 지 확인
ex) esxi-01jaehotest.net 입력 했을때 맞는 ip 나오는지 확인
※ 조심해야하는거 - 기입할때 정확하게 타이핑치기....

#### vcenter 설치 하기
메모리도 많이 잡아먹고 굉장히 무거운 프로그램 이기때문에
8번 서버에서만 운영하는 걸로 

vcenter - pc에서도 운영가능하긴 하지만 네트워크 구성을 다 해줘야 한다.
windows 서버에 cd 넣고 vcenter 올린다 (먼소리지..)

1) window server 접속
2) window server 에 iso 파일 vcenter 넣어주기(vcsa)
3) window server 안에 cd/rom에 vcenter 들어가서 
/This PC/DVD/VMware/vcsa-ui-install/installer 선택

install 선택
- license agree 선택, target = 172.16.0.8
root
VMware1! 입력 next

vm-name = 이름입력(ex jaeho-vcenter)
비밀번호 설정 VMware1!
next

network 그대로
ip version 그대로
FQDN = dns이름 ex) vcenter.jaehotest.net
ip address = vcenter ip (172.16.0.26)
subnet = 24
gateway = 172.16.0.254
dns server = window server ip (172.16.0.25)

 하고 install
 ----------------여기까지 stage1 완료...
 
