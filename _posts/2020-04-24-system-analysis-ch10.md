---
title: "[시스템분석및설계] Chapter 10. 전략(Strategy) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch10. 전략(Strategy) 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-04-24T01:00:00+09:00  
---  

## 01. Strategy


## 02. 예제 프로그램

### 클래스와 인터페이스

### 클래스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-1.png){: width="100%"}

### Hand 클래스

- fight()
	- 현재 손이 입력인자 손과 무승부면 0, 이기면 1, 지면 -1을 반환함
	- 우열 판정하는 수식  
		- 현재, 손이 주먹(0)이고 입력 손이 가위(1)라면
		- 또는, 현재 손이 가위(1)이고 입력 손이 보(2)라면
		- 또는, 현재 손이 보(2)이고, 입력 손이 주먹(0)이라면
		- 현재 손이 이긴다 ⇒ 1을 반환한다
		- 코드에서 this를 생략해도 된다  
	

### Main 클래스
	- 실제로 Player 두 명을 생성해서 가위바위보 게임을 시킴  
	![](https://eliotjang.github.io/assets/images/system-analysis/ch10-2.png){: width="100%"}  





![](https://eliotjang.github.io/assets/images/system-analysis/ch10-3.png){: width="100%"}


## 03. 등장 역할

- 클래스 다이어그램  

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-4.png){: width="100%"}

## 04. 독자의 사고를 넓혀주는 힌트


## 05. 관련 패턴


## 06. 요약


## 연습 문제
- 각자 공부할 것
- <http://www.hyuki.com/dp/>  
![](https://eliotjang.github.io/assets/images/system-analysis/ch10-5.png){: width="70%"}

## 참고자료

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-6.png){: width="100%"}  

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-7.png){: width="100%"}  

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-8.png){: width="100%"}  

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-9.png){: width="100%"}  

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-10.png){: width="100%"}






























