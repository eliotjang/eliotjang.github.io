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


### 주요 클래스  

- BigChar 
  - '큰 문자'를 나타내는 클래스 
  - 파일로부터 큰 문자의 텍스트를 읽어 메모리에 저장하고, print 메소드에서 그것을 표시한다 
  - 큰 문자는 메모리를 많이 차지하므로, BigChar의 인스턴스를 공유하자 
- BigCharFactory 
  - BigChar 클래스의 인스턴스를 만든다 
  - 같은 문자에 대응하는 BigChar 클래스의 인스턴스가 <span style="color:red">이미 만들어져 있는 경우에는, 이것을 이용</span>하고 새로운 인스턴스는 만들지 않는다 
  - 지금까지 만들어진 BigChar 클래스의 인스턴스는 모두 pool이라는 필드에 보관함(HashMap을 이용)  


### BigChar 클래스 

- '큰 문자'를 나타내는 클래스 
- 생성자 
  - 인수로 제공된 문자의 '큰 문자' 버전을 작성한다 
  - 작성된 문자열은 fontdata에 저장함 
  - 예를 들어, 생성자의 인수로 '3'이 주어지면, 다음과 같은 문자열이 fontdata에 저장됨 ⇐ 이 data는 big3.txt에서 읽어온다 
    - 파일로부터 한 라인 씩 읽어서, "\n"과 함께 buf에 추가한다 
    - but.toString()을 이용해서 문자열로 바꾼다음 fontdata에 할당한다  

















