---
title: "[시스템분석및설계] Chapter 5. 싱글턴(Singleton) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch5. 싱글턴(Singleton) 패턴"
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
	- <span style="color:red">프로그래머가 실수를 해도 인스턴스가 1개만 생성되는 것을 보증</span>
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
- Abstract Factory 패턴(8장)
- Builder 패턴(7장)
- Facade 패턴(15장)
- Prototype(6장)

## 06. 요약
- 하나의 인스턴스만 생성되어야 하는 클래스 구현 패턴

## 연습 문제

### 5-1. TickerMaker 클래스에 Singleton 패턴 적용하기
- 해답에서, getNextTicketNumber()를 synchronized로 선언한 이유는?
	- 다수의 스레드가 동시에 이 메소드를 호출하면, 같은 값을 반환할 위험이 있기 때문
	- synchronized 메소드 실행 시, 다른 스레드가 그 메소드를 호출하면, 그 메소드 실행이 완료될 때까지 기다려야 한다.

### 5-2. 인스턴스 개수가 3개로 한정되어 있는 클래스 Triple 만들기
- 배열을 이용한다.

### 5-3. 리스트5-4가 Singleton 패턴이 되지 않는 이유는?
- 구현 방식 설명
	- getInstance() 메소드에서, singleton 필드가 null 이면 Singleton 객체를 생성하고, null이 아니면, 현재 있는 Singleton 객체를 반환한다.
	- null은, 현재 Singleton 객체가 생성되었는지 안되었는지를 나타내는 값으로 사용된다.
- Singleton 패턴이 안되는 이유(다중 스레드 환경인 경우)
	- 두 스레드가 동시에 getInstance()를 호출한 경우 첫 스레드가 (singleton == null)인지 비교해서 '참'이므로 Singleton 객체를 생성하고, 그 객체를 singleton 변수에 할당하려고 한다.
	- 그런데, singleton 변수에 할당하기 직전에, 다른 스레드가 CPU를 할당받고 getInstance()를 호출하여 (singleton == null)을 비교하는 경우가 있다. 아직 singleton 변수에는 Singleton 객체가 할당되어 있지 않으므로, 비교 결과가 또 참이 되어 Singleton 객체를 또 생성하려고한다.
- 결과적으로 두 개 이상의 Singleton 객체가 생성될 수 있다.
- 예: 445~446p
	- 리스트 5-5
		- main()에서 3개의 스레드를 생성하고,
		- run()에서 Singleton.getInstance()를 실행한다.
	- 리스트 5-6
		- 생성자 Singleton()에서, 객체 생성시 일부로 속도를 sleep하고 CPU를 내놓는다.
		- 그 결과 두 개 이상의 인스턴스가 생성될 확률이 높아짐
- 해결책
	- getInstance() 메소드를 synchronized로 선언한다.
		- 동시에 두 개 이상의 스레드가 getInstance() 메소드 안으로 들어오지 않도록 막는다.

## 참고자료
<span style="color:red">Java 8은 스트림 API(java.util.stream)로 기존의 스레드 API로 멀티스레딩 코드 구현 시의 문제점과 멀티코어 활용 어려움을 해결</span>  

![](https://eliotjang.github.io/assets/images/system-analysis/ch05-5.png){: width="100%" height="100%"}

![](https://eliotjang.github.io/assets/images/system-analysis/ch05-6.png){: width="100%" height="100%"}

![](https://eliotjang.github.io/assets/images/system-analysis/ch05-7.png){: width="100%" height="100%"}


































