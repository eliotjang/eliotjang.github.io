---
title: "[네트워크보안] Chapter 00. TCP_IP Protocol"
excerpt: "장상수 저, 정보보호총론, 생능출판, 2015"
toc: true
toc_sticky: true
header:
  teaser: /assets/images/network-security/network-security-logo.jpeg
categories:
  - network security  

tags:
  - network security  

last_modified_at: 2020-03-21T17:00:00+09:00
---  

## 【TCP/IP 참조 모델】  

![](https://eliotjang.github.io/assets/images/network-security/ch00-1.png){: width="60%" height="80%"}  

네트워크 시스템은 복잡하기 때문에 복잡한 시스템의 구조를 명확히 파악하는데 용이하고 시스템을 유지보수하거나 개선하는데 용이하도록 계층화된 모듈로 구성되어 있다.  

### OSI 참조모델 : 7개의 계층으로 구성  
  - <span style="color:black">물리 계층</span>
    - 컴퓨터나 라우터같은 네트워크 장치를 물리 매체에 연결하기 위한 전기/기계적인 인터페이스를 구성하고 있다.
      + 물리 매체(전송 매체)는 유선 매체와 무선 매체로 구분할 수 있다.
      + `유선 매체 : Lan 케이블`, `무선 매체 : Wifi`
    - 케이블 및 커넥터 규격을 포함하고 있으며 데이터 비트들을 전자기 신호로 변환하여 전송하는 방식을 규정하고 있다.
  - <span style="color:black">데이터 링크 계층</span>
    - 네트워크 매체(물리 매체)를 공유하는 노드들 간 데이터를 송수신하기 위한 절차를 규정하고 있다.  
      - `노드 : 컴퓨터, 라우터, 스위치 등`  
    - 이웃한 노드 간 데이터를 송수하기 위해서 framing, 물리 주소 지정, 흐름 제어, 오류 제어, 접근 제어 등의 기능을 수행하고 있다.  
      - 네트워크 매체를 공유하는 각 노드를 식별하기 위한 주소를 물리 주소라고하는데, 다른 말로 하드웨어 주소, 맥 주소라고도 한다.  
  - <span style="color:black">네트워크 계층</span>  
    - 소스 노드에서 목적지 노드로의 패킷 전달을 위한 절차를 규정하고 있다.  
    - 이를 위해 소스 노드와 목적지 노드를 식별하기 위한 논리 주소 지정, 라우팅의 기능을 수행해야 한다.  
  - <span style="color:black">전송 계층</span>  
    - 응용 프로세스와 응용 프로세스 간 메시지의 전송 방식을 규정하고 있다.  
    - 신뢰적인 전송 방식을 적용하는 프로토콜과 비신뢰적인 전송 방식을 적용하는 프로토콜로 구분되어 있다.  
  - <span style="color:black">세션 계층</span>  
    - 두 컴퓨터 간 상호관계를 세션이라고 하는데, 두 컴퓨터간 상호관계 설정, 종류, 절차를 규정하고 있다.  
      - 로그인과 같은 것이 세션 계층에 해당되는 기능이라고 할 수 있다.  
  - <span style="color:black">표현 계층</span>  
    - 응용이 사용하는 데이터의 표현, 제어 방식을 규정하고 있다.  
    - 데이터 압축, 변환, 암호화 등과 같은 프로토콜이 표현 계층에 포함된다.  
  - <span style="color:black">응용 계층</span>  
    - 응용 프로세스간 데이터를 교환하게하는 상호동작을 규정하고 있다.  
    - 자원 공유, 전자 메일 등과 같은 프로토콜이 응용 계층에 포함된다.


### TCP/IP 참조모델 : 5개의 계층으로 구성
  - Host-to-network 계층은 OSI 참조 모델의 물리 계층과 데이터 링크 계층에 해당된다.  
  - <span style="color:black">데이터 링크 계층</span>
    - 링크 : 통신 경로상의 인접한 두 노드들을 연결하는 통신 채널
      - `유선 링크(wired links)`
      - `무선 링크(wireless links)`
    - 프레임 : 데이터링크 계층의 데이터 전송 단위  
  > 데이터 링크 계층은 한 노드에서 인접한 다른 노드로 링크를 따라 프레임을 전달한다.  
    
![](https://eliotjang.github.io/assets/images/network-security/ch00-2.png){: width="296" height="354"}    

  - <span style="color:black">인터넷 계층</span>
    - **네트워크 주소(IP 주소)를 이용하여 패킷을 발신지에서 목적지로 전달하는 책임을 가진다.**
      - source-to-destination delivery
    - 인터넷 계층은 발신지(source) 노드와 목적지(destination) 노드가 다른 네트워크에 있을 때 패킷을 전달하는데 사용되는 프로토콜들을 규정한다.
      - Internet Protocol(IP)
      - Internet Protocol(IPsec) : IP 보안 기능을 추가한 프로토콜
      - Internet Control Message Protocol(ICMP) : 소스 노드에서 목적지 노드로 데이터 패킷을 전달하는 과정에서 오류가 발생할 경우에 그 오류 사실을 소스 노드에 알려줄 때 사용되는 프로토콜
      - Internet Group Management Protocol(IGMP) : 인터넷상에서 멀티캐스트 그룹을 형성해서 데이터를 멀티캐스트할 때 사용되는 그룹 관**
      - **Routing protocols**(OSPF, RIP, BGP 등)
  - <span style="color:black">전송 계층</span>
    - **포트 주소(포트 번호)를 이용하여 응용 프로세스 간 메시지 전달을 담당하는 계층**
      - 응용 계층이 데이터를 송수신할 수 있는 인터페이스 제공
      - TCP, UDP, SCTP
	- SCTP는 최근에 나온 것으로 아직 잘 사용하진 않는다.
  - <span style="color:black">TCP (Transmission Control Protocol)</span>
    - 신뢰성 있고 순서를 보장하는 데이터 전달 기능 제공
    - 흐름 제어 (flow control)
    - 혼잡 제어 (congestion control)
    - 데이터 전송 전 연결 설정 수행(연결지향적)
  - <span style="color:black">UDP (User Datagram Protocol)</span>
    - 비신뢰적이고 데이터 전송 순서를 보장하지 않음 
    - 흐름 제어나 혼잡 제어가 없음
    - 사전 연결 설정과정이 없음(비연결지향적)
  - <span style="color:black">응용 계층</span>
    - 개체들이 네트워크를 통하여 정보를 생성하고 교환할 수 있도록 해주는 응용 환경
      - 다양한 네트워크들이 존재  
	`사람과 사람간 직접적인 통신을 가능하게 하는 응용`  
	`사람과 시스템간의 통신을 가능하게 하는 응용`    
	`시스템과 시스템 사이의 통신을 가능하게 하는 응용`      
    - 응용 계층의 프로토콜들
      - FTP, Telnet, DNS, HTTP 

### 주소 개념


<font color="black">물리 주소</font>
  - 같은 네트워크에 연결된 노드를 식별하기위한 하드웨어 주소
  - 데이터링크 계층의 주소로서 인접한 노드를 식별
  - MAC 주소: 48-bit format  
    > 07:01:02:01:2C:4B  
    > 6-byte (12개의 16진수) 물리주소  


<font color="black">네트워크 주소</font>
  - 하부의 물리적인 네트워크와 독립적으로 통신하기 위하여 사용되는 주소
  - 네트워크 계층의 주소로서 소스 및 목적지 호스트를 식별
  - IP 주소 : 32-bit format


<font color="black">포트 주소</font>
  - 전송 계층의 주소로서 통신하는 응용프로세스를 식별
  - TCP/UDP port 번호 : 16-bit format

![](https://eliotjang.github.io/assets/images/network-security/ch00-3.png){: width="100%" height="60%"}  

소스노드와 목적지노드의 주소는 변함이 없지만, 데이터링크 계층의 맥 주소는 인접한 노드를 거칠때 마다 변경이 된다.  
따라서 네트워크 계층이 하는 일은 소스 호스트에서 목적지 호스트까지 패킷을 전달하는 일을 담당한다고 하는 것이고,  
반면에 데이터 링크 계층이 하는 일은 인접한 노드 간의 데이터 프레임을 전송하는 일을 담당한다고 하는 것이다.  


![](https://eliotjang.github.io/assets/images/network-security/ch00-4.png){: width="80%" height="60%"}  

### 캡슐화 

송신측 패킷 전송 형태  

![](https://eliotjang.github.io/assets/images/network-security/ch00-5.png){: width="60%" height="40%"}  


수신측 패킷 전송 형태  

![](https://eliotjang.github.io/assets/images/network-security/ch00-6.png){: width="60%" height="40%"}  


## 【인터넷 프로토콜(IP)】  

### IP 주소의 일반 형식  
![](https://eliotjang.github.io/assets/images/network-security/ch00-7.png){: width="50%" height="30%"}  

  - 라우터의 개입 없이 상호 데이터 전달이 가능한 영역(e.g. LAN)
  - 같은 네트워크 주소를 갖는 인터페이스들의 집합

![](https://eliotjang.github.io/assets/images/network-security/ch00-8.png){: width="40%" height="40"}  

![](https://eliotjang.github.io/assets/images/network-security/ch00-9.png){: width="40%" height="10%"}  

### 특수 주소  
 
![](https://eliotjang.github.io/assets/images/network-security/ch00-10.png){: width="80%" height="60%"}  

  - 사설망 주소
    - 10.0.0.0 ~ 10.255.255.255
    - 172.16.0.0 ~ 172.31.0.0
    - 192.168.0.0 ~ 192.168.255.255  

  - 대부분의 시스템에서 유효한 루프백 주소는 127.0.0.1

### IP 데이터그램 구조  

![](https://eliotjang.github.io/assets/images/network-security/ch00-11.png){: width="80%" height="60%"}  


## 【User Datagram Protocol】 

  - 비연결형 서비스
    - 송신자와 수신자 간 no handshaking
    - 각 데이터그램은 다른 것과 독립적으로 처리
  - 송신 프로세스와 수신 프로게스 간 비 신뢰적인 데이터 전달
    - 메시지가 손실될 수 있음
    - 응용 프로세스로의 데이터 전달 순서가 보장되지 않음  
  - No flow control  
  - No congestion control  
  - 지원 가능 통신 방식
    - 1-to-1
    - 1-to-many
    - many-to-1
    - many-to-many  

### 왜 UDP를 사용하는가?

연결설정이 없다
  - 연결 설정에 따른 지연 감소  

구현이 간단하다
  - 송신 및 수신 측에서 연결 상태를 관리할 필요가 없다  

헤더의 크기가 작다
  - 작은 오버헤드  

No congestion control
  - 원하는대로 데이터를 빨리 전송할 수 있다  

인터넷 전화, 멀티미디어 스트리밍 서비스, DNS, SNMP, TFTP 등의 응용에서 이용  

### User Datagram Format  

![](https://eliotjang.github.io/assets/images/network-security/ch00-12.png){: width="80%" height="40%"}  

![](https://eliotjang.github.io/assets/images/network-security/ch00-13.png){: width="70%" height="50%"}  

## 【전송제어 프로토콜(TCP)】 

### TCP의 속성 및 특징  

  - 스트림 지향 서비스 제공
    - 송신 프로세스는 연속된 바이트들의 흐름으로 데이터를 전달하며, 수신 프로세스도 바이트들의 흐름으로 데이터를 수신하여 처리
    - TCP는 데이터를 운반하는 가상의 파이프에 의해 2개의 프로세스가 연결되는 것처럼 보이는 환경을 제공
    - 수신자가 데이터를 읽어가는 단위는 송신된 데이터 크기와 독립적
  - 신뢰성
    - 순서 번호에 의한 세그먼트 전달 순서 보장
    - 세그먼트 중복 수신 방지
    - 수신 확인(ACK)에 의한 재전송
  - 흐름제어
    - 송신 호스트의 데이터 전송 속도 조절 수단 제공
  - 3-way handshake에 의한 연결 설정
    - 최초 순서 번호, 수신 윈도우 크기 등의 교환
    - 연결은 소켓을 통해 이루어짐
    - 소켓은 송신 호스트의 IP 주소 및 포트 번호, 수신 호스트의 IP 주소 및 포트 번호에 의해 식별  

### TCP 헤더  

![](https://eliotjang.github.io/assets/images/network-security/ch00-14.png){: width="70%" height="60%"}  

### 3-Way Handshake

![](https://eliotjang.github.io/assets/images/network-security/ch00-15.png){: width="30%" height="50%"}  

### 연결 해제 절차

![](https://eliotjang.github.io/assets/images/network-security/ch00-16.png){: width="30%" height="50%"}  





