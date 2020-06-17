---
title: "[시스템분석및설계] Chapter 22. Command 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch22. Command 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-06-17T22:00:00+09:00  
---  

## 01. Command 패턴  

- 클래스가 일을 처리할 때는,
  - 자신의 클래스나 다른 클래스의 메소드를 호출한다 
- Command 패턴에서는, 실행하고자 하는 일이 
  - 메소드 호출이 아닌, *'명령을 나타내는 클래스'*의 인스턴스 생성으로 표현된다 
- 이점 
  - history를 관리하고 싶을 때, Command 클래스의 인스턴스의 집합을 관리하면 된다 
  - 명령의 집합을 보존해 두면, 똑같은 명령을 재실행 할 수도 있고, 또는 여러 개의 명령을 모든 것을 새로운 명령으로서 재사용할 수도 있다  


## 02. 예제 프로그램  

- 간단한 그림 그리기 프로그램 
  - 마우스를 drag 하면 빨간 점이 연결되어 그림이 그려진다 
  - 'clear' 버튼을 누르면 점이 모두 지워진다 
  - 사용자가 마우스를 드래그할 때 마다, '이 위치에 점을 그려라'라는 명령이 DrawCommand 클래스의 인스턴스로 생성된다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch22-1.png){: width="80%"}  
  - 
|패키지|이름|해설|
|------|----|----|
|command|Command|'명령'을 표현하는 인터페이스|
|command|MacroCommand|'여러 개의 명령을 모은 명령'을 나타내는 클래스|
|drawer|DrawCommand|'점 그리기 명령'을 표현한 클래스|
|drawer|Drawale|'그리기 대상'을 표현한 인터페이스|
|drawer|DrawCanvas|'그리기 대상'을 구현한 클래스|
|Anonymous|Main|동작 테스트용 클래스|  




















