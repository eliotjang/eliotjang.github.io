---
title: "[시스템분석및설계] Chapter 9. 브릿지(Bridge) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch9. 브릿지(Bridge) 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-04-23T23:00:00+09:00  
---  

## 01. Bridge 패턴
- Bridge(다리)

- 클래스 계층의 두 가지 역할

- 새로운 '기능'을 추가하고 싶을 때
	- ㅇ
		- ㅇ  
		![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-1.png){: width="80%"}
	- ㅇ  
	![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-2.png){: width="80%"}
	- d

- 새로운 '구현'을 추가하고 싶을 때
	- ㅇ
	- ㅇ
	- ㅇ
	- ㅇ
		- ㅇ
	- ㅇ  
	![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-3.png){: width="80%"}
	- ㅇ
	- ㅇ  
	![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-4.png){: width="80%"}

- 클래스 계층의 혼재와 클래스 계층의 분리


## 02. 예제 프로그램

- '무언가를 표시하기'위한 프로그램

- 클래스 다이어그램  

![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-5.png){: width="100%"}

- 기능의 클래스 계층: Display 클래스

- 기능의 클래스 계층: CountDisplay 클래스

- 구현의 클래스 계층: DisplayImpl 클래스

- 구현의 클래스 계층: StringDisplayImpl 클래스

- Main 클래스

![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-6.png){: width="80%"}

## 03. 등장 역할

- Abstraction의 역할

- RefinedAbstraction의 역할

- Implementor의 역할

- ConcreteImplementor의 역할

- 클래스 다이어그램  

![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-7.png){: width="90%"}

## 04. 독자의 사고를 넓혀주는 힌트

- 분리해두면 확장이 편해진다
	- ㅇ
		- ㅇ
		- ㅇ
	- 구현을 추가하고 싶으면 구현 클래스 계층을 확장한다
		- 예:
			- Display 생성시, 생  
			![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-8.png){: width="100%"}

- 상속은 견고한 연결, 위임은 느슨한 연결

## 05. 관련 패턴

- Template Method 패턴(3장)
- Abstract factory 패턴(8장)
- Adapter 패턴(2장)

## 06. 요약

- 두 종류의 클래스 계층 사이의 다리를 놓는 Bridge 패턴

- 구현의 클래스 계층에서 제공하는 메소드들을 이용하고 조합하여, 기능 클래스 계층에서 부가적인 기능을 제공한다


## 연습 문제

- 9-1

- 9-2

- 9-3


































