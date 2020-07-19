---
title: "[디자인 패턴] Chapter 6. 프로토타입(Prototype) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch6. 프로토타입(Prototype) 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - 컴퓨터공학
tags:
  - 디자인-패턴
  - java
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

### Product 인터페이스 / Manager 클래스
- Product 인터페이스나 Manager 클래스의 소스에, 구체적인 제품인 MessageBox나 UnderlinePen 클래스의 이름이 전혀 등장하지 않는다.
	- Manager 클래스는, 구체적인 클래스 이름을 사용하지 않고, Product 인터페이스 이름만을 사용한다.
	- framework와 구체적인 클래스를 분리함
	- <span style="color:red">Product나 Manager를, 구체적인 클래스와 상관없이 수정할 수 있다.</span>

### MessageBox 클래스
- Product 인터페이스를 구현
- decochar 필드
	- 문자열을 둘러쌀 장식 문자
- use() 메소드
	- 주어진 문자열을 decochar로 둘러싸서 출력하는 일을 한다.
- createClone() 메소드
	- 자기 자신을 복제하는 메소드 (자신의 clone()을 호출함)
	- clone(): 인스턴스가 가지고 있는 필드 값이 그대로 복사된 복제 인스턴스를 반
환한다.
		- java.lang.Cloneable 인터페이스를 구현한 클래스만이 이 clone() 메소드를
 가진다.
		- 이 인터페이스를 구현하지 않는 경우에는, CloneNotSupportedException이 발생한다.
		- <span style="color:blue">Product 인터페이스는 java.lang.Cloneable 상속
 (p.106)</span>
	- clone() 메소드는, 자신의 클래스(및 하위 클래스)에서만 호출할 수 있다.
		- 다른 클래스의 요청으로 복제하는 경우에는, createClone과 같은 다른 메소
드로 clone을 감싸주어야한다.

```java
import framework.*;

public class MessageBox implements Product {
	private char decochar;
	public MessageBox(char decochar) {
		this.decochar = decochar;
	}
	public void use(String s) {
		int length = s.getBytes().length;
		for (int i=0; i < length+4; i++) {
			System.out.print(decochar);
		}
		System.out.println("");
		System.out.println(decochar + " " + s + " " + decochar);
		for (int i=0; i < length+4; i++) {
			System.out.print(decochar);
		}
		System.out.println("");
	}
	public Product createClone() {
		Product p = null;
		try {
			p = (Product)clone();
		} catch (CloneNotSupportedException e) {
			e.printStackTrace();
		}
		return p;
	}
}
```  

### UnderlinePen 클래스
- MessageBox와 거의 같은 동작을 함
- 문자열에 밑줄을 그어준다.
- ulchar 필드
	- 밑줄 그을 때 사용할 문자를 가지고 있음

```java
import framework.*;

public class UnderlinePen implements Product {
	private char ulchar;
	public UnderlinePen(char ulchar) {
		this.ulchar = ulchar;
	}
	public void use(String s) {
		int length = s.getBytes().length;
		System.out.println("\"" + s + "\"");
		System.out.print(" ");
		for (int i=0; i < length; i++) {
			System.out.print(ulchar);
		}
		System.out.println("");
	}
	public Product createClone() {
		Product p = null;
		try {
			p = (Product)clone();
		} catch (CloneNotSupportException e) {
			e.printStackTrace();
		}
		return p;
	}
}
```

### Main 클래스
1. Manager의 인스턴스를 생성
2. UnderlinePen 인스턴스와 MessageBox 인스턴스를 이름을 붙여서 등록한다.
3. Manager의 create() 메소드를 호출해서, 원하는 '이름'의 제품을 얻어서, 그것의 use()를 실행한다.  

|이름|클래스와 인스턴스의 내용|
|-----------------|---------------------------------------------|
|"strong message"|UnderlinePen에서 ulchar는 '~'|
|"warning box"|MessageBox에서 decochar는 '*'|
|"slash box"|MessageBox에서 decochar는 '/'|

```java
import framework.*;

public class Main {
	public static void main(String[] args) {
		// 준비
		Manager manager = new Manager();
		UnderlinePen upen = new UnderlinePen('~');
		MessageBox mbox = new MessageBox('*');
		MessageBox sbox = new MessageBox('/');
		manager.register("strong message", upen);
		manager.register("warning box", mbox);
		manager.register("slash box", sbox);

		// 생성
		Product p1 = manager.create("strong message");
		p1.use("Hello, world.");
		Product p2 = manager.create("warning box");
		p2.use("Hello, world.");
		Product p3 = manager.create("slash box");
		p3.use("Hello, world.");
	}
}
```  

![](https://eliotjang.github.io/assets/images/system-analysis/ch06-2.png){: width="80%" height="60%"}

## 03. 등장 역할

### Prototype(원형) 역할
- 인스턴스를 복사(복제)해서 새로운 인스턴스를 만들기 위한 메소드를 결정함
- 예제애서는, Product 인터페이스에 해당됨

### ConcretePrototype(구체적인 원형) 역할
- 인스턴스를 복사(복제)해서 새로운 인스턴스를 만드는 메소드를 실제로 구현함
- 예제에서는, MessageBox나 UnderlinePen 클래스가 해당됨

### Client(이용자)의 역할
- 인스턴스를 복사하는 메소드를 이용해서 새로운 인스턴스를 만듬
- 예제에서는, Manager 클래스가 해당됨

![](https://eliotjang.github.io/assets/images/system-analysis/ch06-3.png){: width="80%" height="60%"}

## 04. 독자의 사고를 넓혀주는 힌트

- Prototype 패턴을 사용하지 않으면,
	- 인스턴스 복제(모든 필드 값을 같게 함)시
		- new Something() 한 후에,
		- Something 객체의 모든 필드 값을 기존의 Something 인스턴스로부터 얻어와야 한다.
		- 필드가 priavte이고, 그 필드 값을 얻어오는 메소드가 없다면 어떻게 할 것인가?
			- 복제가 불가능하다.  
			![](https://eliotjang.github.io/assets/images/system-analysis/ch06-4.png){: width="50%"}

## 05. 관련 패턴
- Flyweight 패턴(20장)
- Memento 패턴(18장)
- Composite 패턴(11장) 및 Decorator 패턴(12장)
- Command 패턴(22장)

## 06. 보강: clone() 메소드와 java.lang.Cloneable 인터페이스

- 자바 언어의 clone
	- 인스턴스를 복사하는 장치로 clone()메소드가 제공된다.
	- 복사 대상이 되는 클래스는, 반드시 java.lang.Cloneable 인터페이스를 구현해야 한다.
	- Cloneable 인터페이스를 구현한 클래스의 인스턴스는, clone() 메소드를 호출하면 복사된다.
	- Cloneable 인터페이스를 구현하고 있지 않은 클래스의 인스턴스가 clone()메소드를 호출하면, CloneNotSupportedException 에외가 발생한다.
- clone 메소드는 어디에서 정의되어 있는가
	- 최상위 클래스인 java.lang.Object에 정의되어 있다.
	- 모든 클래스에서 clone 메소드를 상속함
- Cloneable 인터페이스에
	- clone() 메소드가 선언되어 있는 것은 아니다.
	- Cloneable 인터페이스는, clone()에 의해 복사될 수 있다는 표시로만 사용된다
- clone() 메소드는 'shallow copy'를 수행한다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch06-5.png){: width="80%" height="70%"}

## 07. 요약
- 인스턴스 복제를 용이하게 하기 위해서 Prototype 패턴을 이용하라.

## 연습 문제

- 6-1
	- MessageBox와 UnderlinePen 클래스에서 같은 작업을 하는 createClone() 메소드를 따로 두지 말고, 한 곳에 두고 공유시키는 방법은?
- 6-2
	- Object 클래스는 java.lang.Cloneable 인터페이스를 구현하는가?

## 복습: C++의 얕은 복사, 깊은 복사
![](https://eliotjang.github.io/assets/images/system-analysis/ch06-6.png){: width="100%" height="100%"}  

![](https://eliotjang.github.io/assets/images/system-analysis/ch06-7.png){: width="100%" height="100%"}























