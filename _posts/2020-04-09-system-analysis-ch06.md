---
title: "[시스템분석및설계] Chapter 6. 프로토타입(Prototype) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch5. 프로토타입(Prototype) 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-04-09T22:00:00+09:00  
---  

## 01. Prototype 패턴
- 일반적인 인스턴스 생성은, 클래스의 생성자를 호출한다.  
`new Something();`  
	- 인스턴스 생성 시에 반드시 클래스 이름을 지정해야 한다.
- 의도
	- 클로스로부터 인스턴스를 새로 만드는 것이 아니라, 현재 존재하는 인스턴스를 복사(복제)해서 새로운 인스턴스를 만들 필요가 있을 때, 이 작업을 편하게 하기 위해 사용한다.
		- ①종류가 너무 많아 클래스로 정리되지 않는 경우
		- ②클래스로부터 인스턴스 생성이 어려운 경우
		- ③framework와 생성할 인스턴스를 분리하고 싶은 경우
- Prototype
	- 원형, 모범
	- "모범이 되는 인스턴스를 기초로 똑같은 새로운 인스턴스를 만든다"
	- 클래스 안에, 자신을 복사하는 메소드를 두자.
		- 복제(복사): 모든 필드 값이 동일한 인스턴스를 생성함

## 02. 예제 프로그램

- 문자열을 틀에 넣어서 표시하거나 밑줄을 그어 표시

|패키지|이름|해설|
|----------|-----------|----------------------------------------------------|
|framework|Product|추상 메소드 use와 createClone이 선언되어 있는 인터페이스|
|framework|Manager|createClone을 사용해서 인스턴스를 복제하는 클래스|
|Anonymous|MessageBox|문자열을 틀에 넣어 표시하는 클래스.<br/>use와 createClone을 구현하고 있다.|
|Anonymous|UnderLinePen|문자열에 밑줄을 그어 표시하는 클래스.<br/>use와 createClone을 구현하고 있다.|
|Anonymous|Main|동작 테스트용 클래스|

### 클래스 다이어그램
![](https://eliotjang.github.io/assets/images/system-analysis/ch06-1.png){: width="80%" height="70%"}

### Product 인터페이스
- java.lang.Cloneable 인터페이스를 상속함
	- 이를 구현한 클래스는, clone() 메소드를 이용해서 복제를 만들어낼 수 있다.
- use()
	- '사용'을 위한 메소드
	- '사용'이 실제 무엇을 의미하는지는 하위 클래스의 구현이 결정함
- createClone() 메소드
	- 인스턴스를 복제하기 위한 것

```java
package framework;
import java.lang.Cloneable;

public interface Product extends Cloneable {
	public abstract void use(String s);
	public abstract Product createClone();
}
```  

### Manager 클래스
- Product 인터페이스를 이용해서 인스턴스를 복제하는 일을 함
- showcase 필드
	- java.util.HashMap (name-value 쌍들을 저장하고 관리하는 자료구조)
	- Product의 '이름'과 '인스턴스'를 저장함
- register() 메소드
	- 제품의 '이름과'과 제품의 '인스턴스'를 showcase에 저장함
	- 인수로 전달되는 Product형의 proto는 Product 인터페이스를 구현한 클래스의 인스턴스(즉, use나 createClone 메소드를 호출할 수 있는 인스턴스)
- Create() 메소드
	- 등록된 제품의 createClone()을 호출하여 복사본을 만든 다음, 이것을 반환한다.

```java
package framework;
import java.util.*;

public class Manager {
	private HashMap showcase = new HashMap();
	public void register(String name, Product proto) {
		showcase.put(name, proto);
	}
	public Product create(String protoname) {
		Product p = (Product)showcase.get(protoname);
		return p.createClone();
	}
}
```  

### 추가사항
- Product 인터페이스나 Manager 클래스의 소스에, 구체적인 제품인 MessageBox나 UnderlinePen 클래스의 이름이 전혀 등장하지 않는다.
	- Manager 클래스는, 구체적인 클래스 이름을 사용하지 않고, Product 인터페이스 이름만을 사용한다.
	- framework와 구체적인 클래스를 분리함
	- <span style="color:red">Product나 Manager를, 구체적인 클래스와 상관없이 수정할 수 있다.</span>

### MessageBox 클래스


### UnderlinePen 클래스


### Main 클래스


## 03. 등장 역할

### Prototype(원형) 역할


### ConcretePrototype(구체적인 원형) 역할


### Client(이용자)의 역할


## 04. 독자의 사고를 넓혀주는 힌트

### Prototype 패턴을 사용하지 않으면


## 05. 관련 패턴


## 06. 보강: clone() 메소드와 java.lang.Cloneable 인터페이스

### 자바 언어의 clone


### clone 메소드는 어디에서 정의되어 있는가


### Cloneable 인터페이스


### clone() 메소드는 'shallow copy'를 수행

## 07. 요약


## 연습 문제

### 6-1


### 6-2


## 복습: C++의 얕은 복사, 깊은 복사
























