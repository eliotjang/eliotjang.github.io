---
title: "[시스템분석및설계] Chapter 20. Flyweight 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch20. Flyweight 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-06-17T17:00:00+09:00  
---  

## 01. Flyweight 패턴  

- 복싱에서의 '플라이급'을 의미한다 ⇒ 매우 가벼움 
- 가볍다 == 메모리의 사용량이 적다 
- new Something()이 실행되면, Something 클래스의 인스턴스를 보관하기 위한 메모리가 할당된다 
- Flyweight 패턴의 아이디어 
  - 인스턴스를 가능한 한 공유시켜서, 쓸데없이 new를 하지 않도록 한다 
  - 인스턴스가 필요할 때 마다 new를 하는 것이 아니라, 이미 만들어져 있는 인스턴스를 이용할 수 있으면 공유하자  


## 02. 예제 프로그램  

- '큰 문자'를 출력하는 프로그램  

|이름|해설|
|----|----|
|BigChar|'큰 문자'를 나타내는 클래스|
|BigCharFactory|BigChar의 인스턴스를 공유하면서 생성하는 클래스|
|BigString|BigChar를 모아서 만든 '큰 문자열'을 나타내는 클래스|
|Main|동작 테스트용 클래스|  


![](https://eliotjang.github.io/assets/images/system-analysis/ch20-1.png){: width="100%"}  


![](https://eliotjang.github.io/assets/images/system-analysis/ch20-2.png){: width="100%"}















