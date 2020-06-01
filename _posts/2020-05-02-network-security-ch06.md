---
title: "[네트워크보안] Chapter 06. 네트워크 보안 관리"
excerpt: "장상수 저, 정보보호총론, 생능출판, 2015"
toc: true
toc_sticky: true
toc_label: "Ch06. 네트워크 보안 관리"
header:
  teaser: /assets/images/network-security/network-security-logo.jpeg
categories:
  - network security
tags:
  - network security
last_modified_at: 2020-05-02T17:00:00+09:00
---  

## 6.1 네트워크 이해

### 1. TCP/IP 일반

1. OSI 7 Layer와 TCP/IP 비교

- TCP/IP 계층은 4계층 구조를 하고 있으며 OSI 7계층과 TCP/IP 계층 비교와 각 계층 간에 동작하는 어플리케이션을 보여주고 있다

![](https://eliotjang.github.io/assets/images/network-security/ch06-1.png){: width="100%"}

2. TCP/IP 프로토콜 모델

  - ①네트워크 인터페이스(Network Interface)
  - 네트워크 계층에 표준화된 인터페이슬 제공하며 캡슐화를 통해 프레임을 생성하여 전송하는 기능을 수행한다
  - OSI 7 Layer 모델의 물리, 데이터링크 계층에 해당된다  

  - ②인터넷 계층(Internet Layer)
  - 주소 지정과 패킷이 목적지 호스트까지 전달될 수 있도록 라우팅 기능을 수행
  - 목적지 호스트로의 데이터를 전달하기 위하여 32비트의 IP 주소 사용  

  |프토토콜 종류|정의|프로토콜 종류|정의|
  |------------|----|--------------|----|
  |IP(Internet Protocol)|비 연결형 데이터 전달|BGP(Border Gateway Protocol)|상이한 시스템의 경계에 있는 라우터 간에 라우팅 정보 전달|
  |ICMP(Internet Control Message Protocol)|호스트나 라우터의 상황 및 오류에 대한 정보 전달|IDRP(InterDomain Routing Protocol)|상이한 시스템에 있는 라우터 간에 라우팅 정보 전달|
  |IGMP(Internet Group Management Protocol)|멀티캐스팅 지원|ARP(Address Resolution Protocol)|아이피 주소를 이더넷 주소로 확인할 때 사용|  

  - <span style="color:green">OSPF(Open Shortest Path First Protocol), RIP(Routing Information Protocol)은 인터넷 계층 프로토콜이 아니다. 물론 네트워크 계층에서 라우팅을 담당한다</span>
  - <span style="color:green">ARP: 인접한 주소의 맥 주소를 모를 때 ARP를 브로드캐스트하여 알고자하는 맥  주소를 확인한다</span>  


  - ③전송 계층(Transport Layer)
  - 호스트에서 수행되는 응용 프로세스 사용자에게 단대단(end-to-end) 데이터 전달 서비스를 제공하는 기능으로 다음과 같은 전송계층 프로토콜이 있다
    - TCP(Transmission Control Protocol): 신뢰성 있는 연결형 데이터 전달 서비스 제공(오류복구, 혼잡제어 등등)
    - UDP(User Datagram Protocol): 최선형(best effort) 비 연결형 데이터 전달 서비스 제공  

  ![](https://eliotjang.github.io/assets/images/network-security/ch06-2.png){: width="100%"}  


  - ④응용 계층(Application Layer)
  - 사용자에게 전자우편, 원격 로그인, 파일 전달 등의 응용서비스를 제공하는 기능을 한다. OSI 7 Layer 모델의 상위 계층인 응용, 표현 및 세션 계층에 해당된다  

  |프로토콜 종류|정의|
  |-------------|----|
  |HTTP(HyperText Transfer Protocol)|WEB 페이지 전달|
  |SMTP(Simple Mail Transfer Protocol)|전자우편 배달|
  |Telnet|원격 로그인|
  |FTP(File Transfer Protocol)|양방향 파일 전달|
  |DNS(Domain Name System)|호스트 이름을 IP로 변환|
  |NFS(Network File System)|원격 파일 액세스|
  |SNMP(Simple Network Management Protocol)|네트워크 관리|  

  - <span style="color:green">Ping은 응용 계층 프로토콜이 아니다. 유틸리티, 즉 ICMP 프로토콜에 의해 구현된 프로그램이다</span>

### 2. IPv4(Internet Protocol version 4)

1. IP 주소 형식
  - IP 주소는 4byte(32bit)로 구성된 논리적인 주소로, XXX.XXX.XXX.XXX의 형태를 띤다. 예를 들어 210.100.1.100과 같이 '.'로 구분된 4개의 10진수 형태를 갖는다  

  ![](https://eliotjang.github.io/assets/images/network-security/ch06-3.png){: width="100%"}

2. IP 주소 클래스
  - IP 주소는 A, B, C, D, E 클래스(Class)의 다섯 가지 종류가 있다
  - 클래스를 나누는 방법은 IP 주소의 제일 처음 바이트인 선두 1비트가 '0'으로 시작되면 A 클래스, 선두 2비트가 '10'으로 시작되면 B 클래스, 선두 3비트가 '110'으로 시작되면 C클래스, 선두 4비트가 '1110'으로 시작되면 D 클래스, 선두 4비트가 '1111'이면 E 클래스이다  

  ![](https://eliotjang.github.io/assets/images/network-security/ch06-4.png){: width="100%"}  

  - <span style="color:green">네트워크 주소는 A는 1바이트, B는 2바이트, C는 3바이트이다</span>

3. 사설 IP 주소
  - 인터넷과 연결 안하고 개별적으로 구성한 네트워크는 IP 주소를 A, B, C Class 중 아무거나 사용해도 상관없으나, 인터넷에 연결하려면 할당 받은 공인 주소를 사용
  - 인터넷에서는 동일한 IP주소를 사용하면 안되기 때문이다. IP 주소 영역에서 인터넷에서는 사용하지 않는 구역이 있는데, 이 구역을 사설 주소(Private Address)라고 한다
  - 또한 이 주소는 라우팅 되지 않으며 외부에서 접근이 불가하다. 접근하기 위해서는 NAT 서비스를 이용해야 한다
  - <span style="color:green">A, B는 많은 주소가 할당되지 못하고 낭비된다. 클래스로 나눈 주소 지정 방식은 주소 낭비를 초래하기 때문에 Class less 방식으로 바꼈다</span>
  - <span style="color:green">공인 IP 주소는 사설 주소 이외에 인터넷을 통해 라우팅을 하기 위한 주소이다</span>
  - <span style="color:green">NAT(Network Address Translation) 기계를 통해 사설 IP 주소를 공인 IP 주소로 바꿀 수 있다. 외부에서 접근할 수 없으므로 보안 상 이점이 있다  

  |Class|구간|
  |-----|----|
  |A|10.X.X.X|
  |B|172.16.X.X ~ 172.31.X.X|
  |C|192.168.X.X|  


### 3. IPv6(Internet Protocol version 6)

1. IPv6 개요
  - IPv4 프로토콜의 한계점으로 인해 지속적인 인터넷 발전에 문제가 예상되어 이에 대한 대안으로서 IPv6 프로토콜을 제정하였다

2. IPv6 특징
  - ①IP 주소의 확장: IPv6는 128bit 주소 공간을 제공한다. 2<sup>128</sup>이므로 거의 무한대에 가까운 주소 공간이다
  - ②호스트 주소 자동 구성: IPv6 호스트는 IPv6 네트워크에 접속하는 순간 자동적으로 네트워크 주소를 부여받는다
  - ③인증 및 보안 기능: 패킷 출처 인증과 데이터 무결성 및 페이로드 암호화 기능 지원
  - ④이동성: IPv6 호스트는 네트워크의 물리적 위치에 제한 받지 않고 같은 주소를 유지하면서도 자유롭게 이동할 수 있다

3. IPv6 천이전략

  - **㉮IPv4/IPv6 듀얼 스택 방식**
    - IPv4/IPv6 듀얼 스택은 IPv6 노드가 IPv4 전용 노드와 호환성을 유지하는 가장 쉬운 방법이다. IPv6/IPv4 듀얼스택 노드는 IPv4와 IPv6 패킷을 모두 주고받을 수 있는 능력이 있어, IPv4 패킷을 사용하여 IPv4 노드와 직접 호환된다  

  - **㉯터널링(Tunneling) 방식**
    - 터널링은 IPv6/IPv4 호스트와 라우터에서 IPv6 데이터그램을 IPv4 패킷에 캡슐화하여 IPv4 라우팅 영역을 통해 전송하는 방법이다  

  - **㉰헤더 변환 방식**
    - 대부분의 호스트가 IPv6인 환경에서 IPv4 호스트와 통신할 때 유용한 방식
    - IPv6 헤더 중 매핑 가능한 필드들을 IPv4 헤더로 변환하여 전송함

### 4. TCP/UDP Port

- TCP 및 UDP 포트는 인터넷 프로토콜 스위트의 전송 계층 프로토콜 중 TCP나 UDP 등 프로토콜이 사용하는 가상의 논리적 통신 연결단이다
- 각 포트는 번호로 구별되며 이 번호를 포트 번호라고 한다  

|포트번호|서비스|설명|
|--------|------|----|
|20|FTP|●File Transfer Protocol-Datagram<br/>●FTP 연결 시 실제로 데이터를 전송한다|
|21|FTP|●File Transfer Protocol-Control<br/>●FTP 연결 시 인증과 제어를 한다|
|23|Telnet|●텔넷 서비스로, 원격지 서버의 실행 창을 얻어낸다|
|25|SMTP|●Simple Message Transfer Protocol<br/>●메일을 보낼 때 사용한다|
|53|DNS|●Domain Name Service<br/>●이름을 해석하는 데 사용한다|
|69|TFTP|●Trival File Transfer Protocol<br/>●인증이 존재하지 않는 단순한 파일 전송에 사용한다|
|80|HTTP|●Hyper Text Transfer Protocol<br/>●웹 서비스를 제공한다|  

### 5. 네트워크 계층 프로토콜

1. ARP(Address Resolution Protocol)
  - IP 패키 전송을 위해서는 목적지 호스트의 MAC 주소를 알아내어 프레임의 Header에 DA로 설정해야 전송할 수 있다
  - IP는 ARP 프로토콜에 요청하여 IP 주소에 대한 Layer 2 주소(MAC 주소)를 알아내도록 하는데 IP 주소를 MAC 주소로 변환해주는 프로토콜을 ARP라 한다


## 6.2 네트워크 서비스

### 1. 네트워크 서비스 관리

1. DNS(Domain Name System)  

  - **㉮개요**
  - DNS는 TCP/IP 네트워크에서 사용되는 네임 서비스 구조이다. 숫자로 표시되는 인터넷 주소를 우리가 이해하기 쉬운 문자 형태로 사용할 수 있도록 주소 체계를 관리하는 서버를 말한다
  - 컴퓨터의 이름은 마침표에 의해 구분되고 알파벳과 숫자로 구성된 세그먼트의 문자열로 구성되어 있다
  - 예를 들어, 기관별로는 com이면 기업체, edu인 경우는 교육기관, gov인 경우는 정부기관 등으로 나누어져 있다. 국가도메인은 au는 호주, ca는 캐나다, jp는 일본, kr는 한국, tw는 대만, uk는 영국 등이다

  - **㉯Name 서버의 역할 분류**
  - ①Primary Name Server: 자신이 관리하는 zone에 대한 데이터를 자기 시스템의 로컬 파일에서 가져와 서비스를 제공하는 DNS 서버이다
  - ②Secondary Name Server: zone에 대한 정보를 네트워크를 통해서 다른 서버로부터 받아와서 DNS 서비스를 제공하는 서버이다
  - ③Cache: 모든 DNS 서버는 DNS 서비스 결과를 자신의 cache에 저장한다
  - ❖zone의 개념: Domain의 지역적 단위(영역)이며, zone 파일은 DNS 서버 위에 만들어 놓은 database파일이다

2. DHCP(Dynamic Host Configuration Protocol)
  - DHCP는 TCP/IP를 사용하는 노드의 IP 설정을 자동으로 해주는 프로토콜이다
  - DHCP를 사용하면 클라이언트의 IP 주소는 물론 Subnet Mask, Default Gateway, DNS 서버 등 여러 가지를 제공할 수 있다
  - DHCP는 호스트 IP 구성 관리를 단순화하는 IP 표준이다
  - DHCP 표준에서는 DHCP 서버를 사용하여 IP 주소 및 관련된 기타 구성 세부 정보를 네트워크의 DHCP 사용 클라이언트에게 동적으로 할당하는 방법을 제공한다


## 6.3 네트워크 프로토콜

### 1. 라우팅의 종류

1. 정적 라우팅
  - 라우팅 테이블이 정적이며, 소규모 네트워크에 사용 가능

2. 동적 라우팅
  - 라우팅 프로토콜을 이용하여 네트워크 토폴로지 변화에 따라 동적으로 라우터의 라우팅 테이블을 생성하고 갱신

### 2. Intra-domain 라우팅 프로토콜

1. RIP(Routing Information Protocol)
  - 거리벡터 라우팅 알고리즘에 기반하며, RIP 메시지는 UDP  데이터그램을 통해 전송된다
  - UDP 포트 520번을 사용하고, Hop Count를 기반으로 경로를 설정함

2. OSPF(Open Short Path First)
  - 최단 경로를 구하는 Dijkstra 알고리즘에 기반하며, OSPF 메시지는 IP 데이터그램에 실려 전달된다
  - 큰 규모에 적합하도록 설계되어 있다

3. Inter-domain 라우팅 프로토콜
  - BGP(Border Gateway Protocol)
    - BGP는 가장 널리 사용하고 있으며, 현재는 BGP 4를 사용한다. 다른 AS상에 있는 라우터들 사이의 통신 프로토콜이다


## 6.4 무선 네트워크 보안

### 1. 무선 LAN

- **㉮개요**  

|무선랜<br/>규격|최대속도<br/>(Mbps)|채널 대역폭<br/>(MHz)|사용 주파수 대역<br/>(GHz)|주요 특징|
|----|-----|----|----|----|
|802.11b|11|20|2.4|●초기 기술, 느린 전송속도<br/>●DSSS|
|802.11a|54|20|5|●OFDM|
|802.11g|54|20|2.4|●OFDM, DSSS<br/>●2.4GHz 대역 간섭|
|802.11n|600|20/40|2.4/5|●OFDM<br/>●MIMO|
|802.11ac|6900|20/40/80/160|5|●OFDM<br/>●MU-MIMO|  

- **㉯무선랜 취약점**  
  - 무선랜은 타 무선 네트워크에 비해 비교적 저렴하고 손쉬운 공격 도구를 이용하여 불특정 다수에 의한 도청, 네트워크 무력화 시도가 가능하다
  - ①누구나 쉽게 무선으로 네트워크에 접근을 할 수 있는 특성으로, 허가되지 않은 사용자 단말에 의한 공격과 불법적인 AP 접속을 유도하는 등의 위험성 내재
  - ②무선 신호에 대한 도청(엿듣기)과 무선 스캐닝을 통한 신호 분석, 서비스 공격 거부 공격, 암호키 크래킹과 불법적인 AP/단말에 의한 공격이 가능

- **㉰무선랜 보안 표준**
  - 인가된 내부 사용자 접속 통제를 위한 사용자 인증, 무선 구간 데이터 암호화를 위한 무선랜 보안 표준  

  |구분|WEP<br/>(Wired Equivalent Privacy)|WPA<br/>(Wi-Fi Protected Access)|WPA2<br/>(Wi-Fi Protected Access 2)|
  |----|------------|----------|----------|
  |개요|1997년 재정(2003년 삭제)|WEP 방식 보완<br/>(Wi-Fi Alliance)|IEEE 802.11i(2004년) 준수|
  |인증|사전 공유된 비밀키 사용<br/>(64비트, 128비트)|●별도의 인증서버를 이용하는 EAP 인증 프로토콜(802.1x)<br/>●WPA-PSK(사전 공유된 비밀키)|●별도의 인증서버를 이용하는 EAP 인증 프로토콜(802.1x)<br/>●WPA-PSK(사전 공유된 비밀키)|
  |암호화|●고정 암호키 사용(인증키와 동일)<br/>●RC4 알고리즘 사용|●암호키 동적 변경(TKIP)<br/>●RC4 알고리즘 사용|●암호키 동적 변경(CCMP)<br/>●AES 등 강력한 블록 암호 알고리즘 사용|
  |보안성|●64비트 WEP 키는 수분 내 노출<br/>●취약하여 널리 쓰이지 않음|●WEP 방식보다 안전하나 불완전한 RC4 알고리즘 사용|●가장 강력한 보안 기능 제공|  

### 2. 무선랜 보안 주요 위협

1. 도청 및 무선 스캐닝
  - 도청 및 무선 스캐닝 공격은 암호화를 하지 않는 무선신호를 분석하여 다양한 정보(개인정보, 위치정보, VoIP 도청 등)를 불법적으로 유출

2. 물리 계층 서비스 거부 공격
  - AP가 서비스 중인 주파수 대영에 강한 전파를 보내서, 그 주파수 대역에서는 원활한 서비스가 이루어지지 않도록 하는 공격으로, 일반적으로 RF 재밍(Jamming)을 의미

3. MAC 프레임 기반 서비스 거부 공격
  - 무선랜 MAC 프레임 줌 암호화되지 않는 제어 및 관리 프레임을 위조함으로써, 단말과 AP 사이의 정상적인 통신을 어렵게 하거나 접속을 해제하는 공격

4. 인증 프레임 위조를 통한 서비스 거부 공격
  - 무선랜 사용자 인증이 완료되지 않은 상태에서 인증 데이터(802.1X, EAP 관련 메시지)를 위조하여 인증 프로세스를 비정상적으로 종료하거나, 비정상적인 오류를 발생시킴으로써 더이상 인증 프로세스를 진행할 수 없도록 하는 공격

5. WEP/WPA 키 크래킹
  - WEP 프로토콜은 취약점이 다수 존재하며, 대체로 수분 이내에 크래킹이 가능

6. 미허가 AP/단말에 의한 공격
  - 의도적으로 또는 비의도적으로 네트워크 관리자의 보안 정책에 위배되어 설치되어 동작하는 모든 종류의 AP 및 단말에 의한 공격 가능성 존재

7. 불법적인 내부망 연결 AP 또는 외부 AP
  - 내부망에 불법적으로 연결된 AP를 이용하면 공격자가 별다른 어려움 없이 내부망에 손쉽게 접근할 수 있어 내부의 중요한 서버 등의 공격 가능
  - 내부 직원의 단말이 외부 AP에 연결되어 있는 경우 공격 위험 상존

### 3. 무선랜 공격에 대한 대응 기법

1. SSID(Service Set Identifier) 설정을 통한 접속 제한
  - Hidden SSID 설정

2. MAC 주소 인증
  - 무선랜 카드에 부여된 MAC 주소 값을 이용하여 무선랜 단말기와 AP를 인증하는 방식으로 사전에 단말기의 MAC 주소를 이용하여 등록 리스트를 만들어 놓고 접속 요청하는 단말기 존재 여부에 의해서 필터링

3. WEP 인증
  - WEP는 데이터 암호화와 사용자 인증 기능을 제공하며, 사용자 인증 기능은 서로 같은 공유키를 갖는 사용자들을 정상적인 사용자로 인증하여 통신하는 방법을 제공한다

4. 동적 WEP 인증 및 EAP 인증
  - 동적 WEP 인증은 WEP 인증 방식이 고정된 공유키 값을 사용하게 되는 보안상의 문제점을 줄이기 위해서 동적 WEP을 적용하였다


## 6.5 모바일 보안

### 1. BYOD(Bring Your Own Device) 보안

1. 개념
  - 모바일 오피스(Mobile office), 스마트워크(Smart work) 구현을 위한 기반으로 다양한 어플리케이션과의 결합하여 기업의 비즈니스 경쟁력 강화를 위해 필수 인프라로 안착하고 있다
  - 따라서 개인 모바일 기기글 업무에도 사용하는 BYOD(Bring Your Own Device) 트렌드가 주목 받고 있다. 기업의 BYOD 선호는 개인화된 모바일 기기를 업무에 활용함으로써 시간과 장소의 제약 없이 쉽게 업무 지원을 가능하게 하여 업무의 효율성과 편의성을 제공하지만, 기업 보안을 위하여 임직원 개인기기에 대한 관리가 요구된다

2. BYOD 보안 위협  
    1. 관리 대상 기기의 다양성으로 체계적 관리 방안이 부재하다
	- 기기 플랫폼, 크기, 기능 면에서도 개인적 선호도 차이로 인해 기기의 표준화가 어렵고 기업이 직접 지원하기에도 경제적으로 부담된다

    2. 기존 보안대첵이 차단하지 못하는 데이터의 유출 위협이 증가한다
	- 개인 모바일 기기들이 업무 환경으로 침투하면서 기업 데이터 접근이 용이하게 되어 기밀 데이터의 외부 유출 가능성이 증가한다

    3. 개인/기업 정보 및 위치 정보 등 유출 위협이 있다
	- 메일, 메모, 일정 등과 같이 데이터 속성 자체가 개인 정보이면서 동시에 업무와 연계성이 있는 경우 기업의 데이터 관리 기준에 모호성이 있어 데이터 유출 위협이 있다

3. BYOD 보안기술

    1. MDM(Mobile Device Management)
	- MDM은 통상 IT 부서가 기기를 완전히 제어할 수 있도록 직원의 스마트패드와 스마트폰에 잠금·제어·암호화·보안정책 실행을 할 수 있는 기능을 제공한다
	- MDM이 적용된 스마트폰에서는 기업의 보안 정책에 위배되는 앱은 설치 및 구동이 불가하다

    2. 컨테이너화(Containerization)
	- 프라이버시 요구가 커짐에 따라 하나의 모바일 기기 내에 업무용과 개인용 영역을 구분해 보안 문제와 프라이버시 보호를 동시에 해결하려는 기술이다
	- 컨테이너라는 암호화된 별도 공간에 업무용 데이터와 개인용 데이터를 분리하고 관리한다

    3. 모바일 가상화(Hypervisors)
	- 하나의 모바일 기기에 개인용과 업무용 운용체계(OS)를 동시에 담아 개인과 사무 정보를 완전히 분리한다

    4. MAM(Mobile Application Management)
	- MAM은 스마트 기기 전체가 아니라 기기에 설치된 업무 관련 앱만 보안 및 관리 기능을 적용한다

    5. NAC(Network Access Control)
	- 사용자 기기가 기업 내부 네트워크 접근 전 보안정책을 준수했는지 여부를 검사하여 비정상 접근 여부에 따라 네트워크 접속을 통제하는 기술이다

### 2. 모바일 오피스 보안

1. 모바일 오피스 구성  

![](https://eliotjang.github.io/assets/images/network-security/ch06-5.png){: width="100%"}

2. 모바일 보안 주요 위협

    1. 단말기 주요기능 조작을 통한 개인정보 및 업무 정보 침해  
	- 공격자는 단말기의 카메라, 마이크 등의 하드웨어 자원 제어 권한을 획득하여 사용자의 사생활 침해 및 업무 활동을 감시할 수 있다  
	![](https://eliotjang.github.io/assets/images/network-security/ch06-6.png){: width="100%"}  

    2. 취약한 무선 네트워크 사용에 따른 정보유출  
	- A씨가 접속한 무선 AP는 사내에서 설치한 무선랜과 동일한 네트워크 이름(SSID)을 가지고 있는 공격자의 가짜(Fake) AP로, A씨가 무선랜을 통해 전송하는 모든 업무정보를 도청 및 탈취한다  
	![](https://eliotjang.github.io/assets/images/network-security/ch06-7.png){: width="100%"}  

    3. 권한 상승을 통한 악성코드 배포  
	- 공격자는 악성코드를 이용해 단말기의 관리자(Root) 권한을 획들할 수 있으며, 이를 통해 공격자는 단말기 내 모든 기능을 제어할 수 있다  
	![](https://eliotjang.github.io/assets/images/network-security/ch06-8.png){: width="100%"}  
    4. 스미싱을 통한 인증정보 탈취  
	- 피싱·파밍의 대표적인 공격으로 스미싱(Smishing)을 통한 악의적인 어플리케이션 설치 유도 및 정보 유출이 이루어질 수 있다  
	![](https://eliotjang.github.io/assets/images/network-security/ch06-9.png){: width="100%"}  
    5. 웹 어플리케이션 기반 악성코드 감염 공격(XSS 공격)  
	- 모바일 오피스 어플리케이션의 약 75% 이상은 웹 어플리케이션 형태로 구현되어 있기 때문에 스크립트 변조가 이루어질 수 있으며, 이를 통한 단말기 악성코드 감염 및 전파  
	![](https://eliotjang.github.io/assets/images/network-security/ch06-10.png){: width="100%"}  
    6. 테더링(Tethering) 사용에 의한 업무용 PC공격  
	- 테더링으로 연결된 PC는 A사의 물리적 보안 장치나 보안 정책으로 통제되지 않은 네트워크 채널을 생성하게 되고 허가 받지 않은 웹하드 사이트에 접속할 수 있게 된다  
	![](https://eliotjang.github.io/assets/images/network-security/ch06-11.png){: width="100%"}  
    7. 키로거를 통한 업무정보 유출  
	- 공격자는 사용자가 메일을 확인하도록 업무와 관련 있는 청구서, 결제 문서 등으로 위장한 이메일을 전송한다. ID와 Password 등의 인증 정보뿐 아니라, 업무를 위한 금융 거래정보, 고객정보 등 모바일 오피스를 이용하면서 사용자가 입력하는 모든 정보들이 유출될 수 있다  
	![](https://eliotjang.github.io/assets/images/network-security/ch06-12.png){: width="100%"}















