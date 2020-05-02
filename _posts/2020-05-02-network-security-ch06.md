---
title: "[네트워크보안] Chapter 06. 네트워크 보안 관리"
excerpt: "장상수 저, 정보보호총론, 생능출판, 2015"
toc: true
toc_sticky: true
toc_label: "Ch05. 네트워크 보안 관리"
header:
  teaser: /assets/images/network-security/nerwork-security-logo.jpeg
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

## 6.2 네트워크 서비스


## 6.3 네트워크 프로토콜


## 6.4 무선 네트워크 보안


## 6.5 모바일 보안































