---
title: "[시스템분석및설계] Chapter 5. 싱글톤(Singleton) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sitcky: true
toc_label: "Ch5. 싱글톤(Singleton) 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-04-09T20:00:00+09:00  
---  

## 01. Singleton 패턴
- 프로그램 실행시,
	- 하나의 클래스에 대한 인스턴스 보통 여러 개 생성된다.
- 반드시 하나의 인스턴만 생성되어야 하는 클래스도 있다.
	- 예: 컴퓨터 자체를 표현한 클래스, 윈도우 시스템을 표현한 클래스
- 이 경우, 프로그래머가 new MyClass()를 한번만 실행하면 된다.
	- 그러나, 1개의 인스턴스만 생성되도록 프로그램에 표현하고 싶다면, Singleton 패턴을 사용한다.

## 02. 예제 프로그램

|이름|해설|
|-------|---------------------------|
|Singleton|인스턴스가 하나만 존재하는 클래스|
|Main|동작 테스트용 클래스|

![](https://eliotjang.github.io/assets/images/system-analysis/ch05-1.png){: width="80%" height="30%"}

### Singleton 클래스 (리스트5-1)
- private static singleton
	- Singleton 클래스의 인스턴스를 생성한다.
	- 정적 필드
		- Singleton 클래스를 로드할 때 <span style="color:red">한 번만</span> 실행된다.
	- priavte이므로, 외부에서 접근하지 못한다.
- private Singleton() 생성자
	- priavte 메소드이므로, 외부에서 new Singleton()을 호출하지 못함.
	- <span sytle="color:red">프로그래머가 실수를 해도 인스턴스가 1개만 생성되는 것을 보증</span>
- public static Singleton getInstance()
	- Singleton 클래스의 유일한 하나의 인스턴스를 얻을 때 사용하는 메소드
	
```java
public class Singleton {
	private static Singleton singleton = new Singleton();
	private Singleton() {
		System.out.println("인스턴스를 생성했습니다.");
	}
	public static Singleton getInstance() {
		return singleton;
	}
}
```  

### Main 클래스 (리스트5-2)
- Singleton.getInstance()를 이용하여, Singleton 클래스의 객체를 얻어온다.  

![](https://eliotjang.github.io/assets/images/system-analysis/ch05-2.png){: width="70%" height="50%"}

```java
public class Main {
	public static void main(String[] args) {
		System.out.println("Start.");
		Singleton obj1 = Singleton.getInstance();
		Singleton obj2 = Singleton.getInstance();
		if (obj1 == obj2) {
			System.out.println("obj1과 obj2는 같은 인스턴스입니다.");
		} else {
			System.out.println("obj1과 obj2는 같은 인스턴스가 아닙니다.");
		}
		System.out.println("End.");
	}
}
```  

![](https://eliotjang.github.io/assets/images/system-analysis/ch05-3.png){: width="80%" height="30%"}


## 03. 등장 역할

### Singleton의 역할
- 유일한 인스턴스를 얻기위한 static 메소드 가짐
- 이 메소드는 언제나 동일한 인스턴스를 반환  
![](https://eliotjang.github.io/assets/images/system-analysis/ch05-4.png){: width="80%" height="30%"}

## 04. 독자의 사고를 넓혀주는 힌트

### 왜 제한할 필요가 있는가
- 인스턴스가 하나만 존재한다는 것이 보증되면, 인스턴스 상호 간에 영항을 주어 생각지 못한 버그가 발생할 가능성이 없어진다.

### 유일한 하나의 인스턴스는 언제 생성되는가
- 프로그램 실행 후, 처음으로 Singleton.getInstance()메소드가 호출되면, Singleton 클래스가 초기화되고, 이 때 static 필드인 singleton 필드가 초기화된다.

## 05. 관련패턴


## 06. 요약


## 연습 문제

### 5-1. TickerMaker 클래스에 Singleton 패턴 적용하기

### 5-2

### 5-3. 리스트5-4가 Singleton 패턴이 되지 않는 이유는?

## 참고자료




