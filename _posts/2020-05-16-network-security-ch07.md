---
title: "[네트워크보안] Chapter 07. 네트워크 기반 공격 기술"
excerpt: "장상수 저, 정보보호총론, 생능출판, 2015"
toc: true
toc_sticky: true
toc_label: "Ch07. 네트워크 기반 공격 기술"
header:
  teaser: /assets/images/network-security/nerwork-security-logo.jpeg
categories:
  - network security
tags:
  - network security
last_modified_at: 2020-05-16T21:00:00+09:00
---  

## 7.1 네트워크 관리도구 및 서비스

### 1. 네트워크 관리도구 및 서비스

1. Ping
    - 상대방 컴퓨터, 네트워크 장비, 서버 장비까지 통신이 잘 되는지를 확인하는 명령이다  
    ![](https://eliotjang.github.io/assets/images/network-security/ch07-1.png){: width="100%"}

2. traceroute
    - 최종 목적지 컴퓨터(서버)까지 중간에 거치는 여러 개의 라우터에 대한 경로 및 응답속도를 표시해준다  
    ![](https://eliotjang.github.io/assets/images/network-security/ch07-2.png){: width="100%"}

3. Netstat
    - 네트워크 상태 확인 도구
    - 웹으로 프로그램을 개발할 때 외부에서 통신이 안되면 사용 포트를 확인할 때가 있다  
    ![](https://eliotjang.github.io/assets/images/network-security/ch07-3.png){: width="100%"}

4. TCPDUMP
    - TCPDUMP는 네트워크 모니터링 및 패킷 분석을 위해 가장 많이 사용되면서 모든 모니터링 및 패킷 분석 툴의 모태가 된다  
    ![](https://eliotjang.github.io/assets/images/network-security/ch07-4.png){: width="100%"}


## 7.2 네트워크 보안 주요 위협

### 1. 비인가 침입시도

1. 위협 요인
    - 접근통제가 취약한 네트워크를 통해 그림과 같이 비인가 접근 시도가 성공할 경우 주요 웹 및 DB 서버에 접근하여 중요정보 및 개인정보를 유출시킬 수 있다  
    ![](https://eliotjang.github.io/assets/images/network-security/ch07-5.png){: width="100%"}  

### 2. 유해 트래픽의 전송

1. 위협 요인
    - 침입차단, 침입탑지시스템 등의 정보보호시스템은 네트워크를 통해 전송되는 웜·바이러스 등의 악성코드를 차단하는 데 한계가 있으며, 이로 인해 정보유출이나 네트워크의 가용성이 침해될 수 있다
2. 위협 대응방안
    - 서버 및 네트워크 자원에 대한 다양한 형태의 침입행위를 실시간 탐지, 분석 후 비정상적인 패킷을 차단하여야 한다
    - 패킷 실시간 분석과 학습을 통해 알려지지 않은 공격에 대응할 수 있는 기능과 시스템 장애 시에도 네트워크서비스의 중단을 방지할 수 있는 Fallover 기능을 제공하는 침입 방지 시스템등을 운영해야 한다

### 3. 잘못된 네트워크 설정
    
1. 불안정한 구조
    - 잘못된 네트워크는 허가 없는 사용자들이 시스템에 침입할 수 있게 해주는 주요 시작점이다
2. 브로드캐스트 네트워크
    - 허브(hub)나 라우터는 수신자 노드가 데이터 패킷을 받아서 프로세스할 때까지 데이터 패킷을 브로드캐스트 한다
3. 중앙 집중형 서버
    - 중앙 집중형 서버를 사용하는 경우 만일 중앙 서버가 손상되면 네트워크 전체가 정지되거나 또는 데이터 조작이나 도난당하기 쉽다


## 7.3 네트워크 기반 공격 기술

### 1. DoS(Denial of Service)/DDoS(Distributed Denial of Service) 공격

1. DoS 특징
    - 서버의 성능을 크게 떨어뜨리거나 서버를 정지시키는 방법을 통해 서버의 정상적인 작동을 방해하는 공격
    - Resource 고갈형
        - 시스템이 가지고 있는 자원을 고갈시켜 정상적인 작동을 하지 못하도록 하는 공격으로 SYN flooding과 같은 공격이 이 범주에 속한다
    - OS 또는 서버 프로그램의 취약점을 이용한 공격
        - Land attack, 죽음의 ping(Ping of Death), teardrop attack 등이 이러한 범주에 속하는 공격인데 이러한 공격은 주로 서버나 OS 자체의 취약점을 공격하여 서버 또는 기계 자체를 다운시키는 공격 기법이다
    - Route 조작형
        - 이러한 유형의 공격으로는 ICMP Router Discovery Attack이 대표적이다
        - 타깃이 되는 서버나 OS를 직접 공격하는 것이 아니라 클라이언트에서 타깃으로 가는 경로를 조작하여 클라이언트가 제대로 서비스를 받지 못하도록 하는 공격 기법이다
    - Bandwidth 잠식형
        - 앞의 공격들과 달리 이러한 유형의 공격은 특별한 대비책은 존재하지 않는 반면 OS나 서버의 취약점과는 관련이 적어 거의 모든 시스템을 공격할 수 있다는 특성을 가지고 있는 아주 강력한 공격 기법이다

2. DDoS 특징
    1. DDoS 공격의 동작 원리
	- 수백 혹은 수천 개의 좀비 시스템(공격자가 사전에 공격 도구를 설치해 놓은 일반 인터넷 사용자들의 시스템)들을 이용해서 공격의 목적이 되는 목표 시스템(혹은 Victim 시스템)을 공격하는 형태를 가진다
    2. DDoS 공격의 형태
	- ①대역폭 공격(Bandwidth Attacks): 엄청난 양의 패킷을 전송해서 네트워크의 대역폭이나 장비 자체의 리소스를 모두 소진시키는 형태이다
	- ②어플리케이션 공격(Application Attacks): TCP와 HTTP 같은 프로토콜을 이용해서 특정한 반응이 일어나는 요청 패킷을 발송하여 해당 시스템의 연산처리 리소스를 소진시켜서 정상적인 서비스 요쳥과 처리를 할 수 없는 상태로 만드는 것이다
    3. DoS(Denial of Service) 공격 기법 종류
	- ①Ping of Death
            - Ping of Death(죽음의 ping) 공격은 몇몇 OS들이 IP fragment를 제대로 재조합하지 못하여 취약점을 이용한 공격 기법이다
                - ping 명령을 보낼 때 패킷을 최대한 길게 하여(65,535바이트 이상) 공격대상으로 보낸다
                - 목적지 호스트가 IP fragment들을 재조합할 때 IP 데이터그램의 최대 크기를 넘어서는 경우 버퍼 오버플로우가 발생돼 시스템이 다운되거나 재부팅될 수 있다
        - ②SYN Flooding attack
            - 공격자가 송진자 IP 주소를 존재하지 않거나 다른 시스템의 IP 주소로 위장하여 목적 시스템으로 SYN 패킷을 연속해서 보내는 방법이다
           - TCP 서버는 동시에 진행할 수 있는 연결들의 개수가 제한되어 있는데 이것을 초과할 경우 정상적ㅇ니 연결 요청을 처리할 수 없다  
		![](https://eliotjang.github.io/assets/images/network-security/ch07-6.png){: width="100%"}
        - ③Smurf Attack
            - 공격자가 공격대상의 IP 주소로 위장하여, 중계 네트워크(Intermediary Network)에 ICMP Echo Request 패킷을 directed broadcast 주소로 전송하는 방법이다  
	    ![](https://eliotjang.github.io/assets/images/network-security/ch07-7.png){: width="100%"}  
        - ④Land Attack
            - 송신자 IP 주소와 수신자 IP 주소, 송신자 포트와 수신자 포트가 동일한 조작된 SYN 패킷을 공격 대상에 전송한다
        - ⑤Teardrop Attack
            - IP 데이터그램 fragment들의 offset 번호의 혼동을 주어 시스템의 패킷 재조합에 과부하가 걸리도록 함으로써 시스템을 못쓰게 하는 공격
	    
    4. DDoS(Distributed Denial of Service) 공격 기법 종류
        - ①Trin
             - 제일 처음 나타난 DDOS 공격 툴로서, 사용하는 DOS 공격 형태는 UDP Packet Flooding이다
             - `Intruder-287655/tcp→Handler`
             - `Handler-27444/udp→Agent`
             - `Agent-31335/udp→Handler`
        - ②TFN(Tribe Floodingg Network)
             - 사용하는 DOS 공격 형태는 UDP, TCP, SYN Flooding, ICMP flooding, Smurf 등이다. TFN은 trinoo와 거의 유사한 분산 서비스거부공격 도구로 많은 소스에서 하나 혹은 여러 개의 목표 시스템에 대해 서비스거부 공격을 수행한다
        - ③TFN2K
             - 거의 대부분이 TFN과 비슷하고 TFN의 업그레이드 버전이라 생각하면 된다
        - ④Stacheldraht
             - 마스터와 에이전트 간 통신을 암호화
             - 공격 프로그램이 자동 업데이트 되도록 설계됨
        - 마스터(master) 시스템: 해커로부터 공격 명령을 받아 에이전트 시스템에 공격 명령을 전달하는 시스템
        - 핸들러 프로그램(handler program): 마스터 시스템에서 위의 일을 하는 프로그램
        - 에이전트(agent) 시스템: 실제 공격을 수행하는 시스템
        - 데몬(daemon) 프로그램: 공격을 수행하는 프로그램(에이전트 시스템에 설치)
        
        - ⑤IP Fragmentation
            - IP 단편화는 IP 데이터그램이 네트워크를 통해 전송될 때, 전송되는 IP 데이터그램의 크기가 해당 전송매체에서 전송될 수 있는 최대 크기 즉, MUT(Maximum Transmission Unit)보다 클 경우 발생한다
        - ⑥NTP(Network Time Protocol)는 사용자의 기기가 정확한 시간을 맞추기 위한 프로토콜로 NTP DDoS 공격은 monlist라는 최근 접속한 시스템의 목록(최대 600개)을 요청하는 명령어를 사용한 공격이다

    5. DoS/DDoS 공격 대응 방법
        - 취약점을 갖는 운영체제 및 유틸리티 프로그램들에 대한 패치
        - 위장된 소스 IP 주소 필터링 등

### 2. Spoofing

1. IP Spoofing
  - IP Spoofing은 신뢰관계에 있는 두 개의 호스트 사이에서 하나의 시스템을 마비시킨 후 마치 자신이 신뢰관계에 있는 호스트인 것처럼 속이는 것을 말한다
    - 유닉스 계열에서는 트러스트 인증법을 사용
    - 트러스트 설정을 해주려면 유닉스에서는 /etc/host.equiv 파일에 다음과 같이 클라이언트의 IP와 접속 가능한 아이디를 등록해 주어야 한다  
![](https://eliotjang.github.io/assets/images/network-security/ch07-8.png){: width="100%"}

2. ARP Spoofing
  - 로컬에서 통신하고 있는 서버와 클라이언트의 IP 주소에 대한 MAC 주소를 공격자의 MAC 주소로 속여 클라이언트가 서버로 가는 패킷이나 서버에서 클라이언트로 가는 패킷이 공격자에게 향하도록 한다  
![](https://eliotjang.github.io/assets/images/network-security/ch07-9.png){: width="80%"}

3. DNS Spoofing
  - 공격 대상이 DNS 서버에 도메인 이름에 대한 IP 주소를 질의할 때 공격자가 실제 DNS 서버보다 빨리 공격 대상에게 DNS 응답(Response) 패킷을 보내 공격 대상이 잘못된 IP 주소로 이름 해석을 하도록 하여 잘못된 웹 접속을 유도하는 공격이다  
![](https://eliotjang.github.io/assets/images/network-security/ch07-10.png){: width="100%"}


### 3. Session Hijacking

- 세션(Session): 사용자와 컴퓨터, 또는 두 대의 컴퓨터 간의 활성화된 상태
- 세션 하이재킹: 세션, 즉 로그인(Login)된 상태를 가로채는 것을 뜻함
- TCP 세션 하이재킹
  - TCP가 가지는 고유한 취약점을 이용해 정상적인 접속을 가로채는 방법
  - 대부분의 인증은 TCP 세션이 시작할 때 이루어지므로 해커는 서버에 대한 접근 권한을 얻을 수 있음
  - 세션 하이재킹 순서
    - ①공격자는 네트워크 스니핑이 가능해야 함(희생자와 같은 네트워크에 위치)
    - ②패킷의 흐름을 모니터링
    - ③순서번호 예측
    - ④희생자 컴퓨터에 대한 연결 해지
    - ⑤세션을 가로챔

- 세션 하이재킹 공격에 대한 대응책
  - SSH와 같이 세션에 대한 인증 수준이 높은 프로토콜을 이용해서 서버에 접속해야 함
  - 클라이언트와 서버 사이에 MAC 주소를 고정시켜주는 줌

![](https://eliotjang.github.io/assets/images/network-security/ch07-11.png){: width="100%"}


### 4. 네트워크 스니핑(Sniffing)

1. 스니핑 개념
  - 네트워크상의 데이터를 도청하는 행위를 뜻한다. LAN에서의 스니핑은 Promiscuous Mode에서 작동한다
  - 네트워크에 접속하는 모든 시스템은 설정된 IP 주소 값과 고유한 MAC 주소 값을 가지고 있음
  - 네트워크 카드에 인식된 2계층과 3계층 정보가 자신의 것과 일치하지 않는 패킷은 무시함  
![](https://eliotjang.github.io/assets/images/network-security/ch07-12.png){: width="80%"}

2. 스니핑 공격 유형
  1. Switch Jamming
    - 스위칭 재밍 공격은 위조된 MAC 주소를 지속적으로 네트워크로 흘려보내 스위치의 주소 테이블의 기능을 마비시키는 공격(MACOF 공격이라고도 함)
    - 스위치에 랜덤한 형태로 생성한 MAC을 가진 패킷을 지속적으로 보내면, 스위치의 MAC 테이블을 저장 용량을 넘게 되고, 스위치의 원래 기능을 잃고 더미 허브처럼 작동하게 되어 스니핑이 가능해진다
  2. ARP Redirect 공격
    - ARP spoofing의 일종으로 공격자가 자신이 라우터라고 속여 외부로 나가는 패킷을 가로채는 공격 기법이다  
![](https://eliotjang.github.io/assets/images/network-security/ch07-13.png){: width="80%"}
  3. ARP Spoofing 공격
    - ARP Redirect 공격과 마찬가지로 공격자 호스트로 들어오는 트래픽을 원래의 호스트로 relay 해주어야만 호스트 간에 정상적 연결을 할 수 있고 스니핑도 할 수 있다. 그렇지 않을 경우 호스트 간의 연결은 이루어질 수 없게 된다  
![](https://eliotjang.github.io/assets/images/network-security/ch07-14.png){: width="80%"}
  4. ICMP Redirect 공격
    - ICMP Redirect 메시지가 어떻게 호스트의 라우팅 경로를 바꿔주는지 보여주는 좋은 예이다  
![](https://eliotjang.github.io/assets/images/network-security/ch07-15.png){: width="80%"}

3. 스니퍼 탐지 방법
  - Ping을 이용한 스니퍼 탐지
    - 의심이 가는 호스트에 ping을 보내면 되는데, 네트워크에 존재하지 않는 MAC 주소를 위장하여 보냄
    - 만약 ICMP Echo Reply를 받으면 해당 호스트가 스니핑을 하고 있는 것임  
![](https://eliotjang.github.io/assets/images/network-security/ch07-16.png){: width="100%"}

  - ARP를 이용한 스니퍼 탐지
    - ping과 유사한 방법으로, 위조된 ARP Request를 보냈을 때 ARP Response가 오면 프러미스큐어스 모드로 설정되어 있는 것
  - DNS를 이용한 스니퍼 탐지
    - 일반적으로 스니핑 프로그램은 사용자의 편의를 위해 스니핑한 시스템의 IP 주소에 DNS에 대한 이름 해석 과정(Inverse-DNS lookup)을 수행
    - 테스트 대상 네트워크로 Ping Sweep을 보내고 들어오는 Inverse-DNS lookup을 감시하여 스니퍼를 탐지
  - 유인(decoy) 방법
    - 가짜 ID와 패스워드를 네트워크에 계속 뿌리고 공격자가 이 ID와 패스워드를 이용하여 접속을 시도할 때 스니퍼를 탐지
  - ARP watch를 이용한 방법
    - ARP watch는 MAC 주소와 IP 주소의 매칭 값을 초기에 저장하고 ARP 트래픽을 모니터링하여, 이를 변하게 하는 패킷이 탐지되면 관리자에게 메일로 알려주는 툴
    - 대부분의 공격 기법이 위조된 ARP를 사용하기 때문에 쉽게 탐지할 수 있음

### 5. 스턱스넷 및 나이트 드래곤

1. 스턱스넷(Stuxnet)
  - 국가 및 산업의 중요 기반 시설을 제어하는 SCADA(Supervisory Control And Data Acquisition) 시스템을 대상으로 한 웜이다
  - 전파를 위해 윈도우 서버 서비스의 취약점을 이용해 공유 폴더를 공격했으며 윈도우 쉘 .Ink(바로가기) 취약점을 이용해 USB를, 윈도우 프린트 스플러 서비스의 취약점인 공유 프린터를 전파 개체로 활용했다

2. 나이트 드래곤(Night Dragon)
  - 2011년 2월 10일 미국의 윌스트리트 저널의 기사를 통해 글로벌 에너지 업체들을 대상으로 한 악성코드를 이용한 보안 위협이 발생한 것이 공개되었다. 해당 보안 위협은 미국 보안 업체 맥아피(McAfee)에 의해 발견되었으며 나이트 드래곤(Night Dragon)으로 명명되었다


### 6. 중간자 공격(Man in the middle attack)

![](https://eliotjang.github.io/assets/images/network-security/ch07-17.png){: width="100%"}






