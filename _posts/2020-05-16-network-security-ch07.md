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
	    Ping of Death(죽음의 ping) 공격은 몇몇 OS들이 IP fragment를 제대로 재조합하지 못하여 취약점을 이용한 공격 기법이다
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
	    













