---
title: "[디자인 패턴] Chapter 9. 브릿지(Bridge) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch9. 브릿지(Bridge) 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - 컴퓨터공학
tags:
  - 디자인-패턴
  - java

last_modified_at: 2020-04-23T23:00:00+09:00  
---  

## 01. Bridge 패턴
- Bridge(다리)
	- 두 장소를 연결하는 역할
	- '기능의 클래스 계층'과 '구현의 클래스 계층' 사이에 다리를 놓는다  

- 클래스 계층의 두 가지 역할
	- 기능의 클래스 계층
	- 구현의 클래스 계층  

- 새로운 '기능'을 추가하고 싶을 때
	- Something 클래스에 새로운 기능을 추가하려고 할 때
		- Something 클래스의 하위 클래스를 새롭게 만든다  
		![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-1.png){: width="80%"}
		- 소규모의 클래스 계층이 발생함 ⇒ '기능의 클래스 계층'
	- SomethingGood 클래스에 다시 새로운 기능을 추가하려면..  
	![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-2.png){: width="80%"}
	- 새로운 기능을 추가하고 싶을 때 클래스 계층 안에서 새로 만들려고하는 클래스와 유사한 클래스를 찾아내, 하위 클래스를 만들어 기능을 추가한다  

- 새로운 '구현'을 추가하고 싶을 때
	- 추상 클래스는, 일련의 메소드들을 추상 메소드로 선언하고, 인터페이스(API)를 규정한다
	- 하위 클래스 쪽에서 그 추상 메소드를 실제로 구현한다
	- 상위 클래스는 추상 메소드로, 인터페이스를 규정하는 역할을 한다
	- 상위 클래스와 하위 클래스의 역할 분담에 의해 부품으로서의 가치(교환 가능성)가 높은 클래스를 만들 수 있다
		- 이유: 상위 클래스를 구현한 하위 클래스들끼리의 API는 통일되므로
	- 이 때에도 클래스 계층이 등장한다  
	![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-3.png){: width="80%"}
	- 이를 '구현의 클래스 계층'이라고 한다
	- AbstractClass의 다른 구현을 만들고 싶으면, 다른 하위 클래스를 만들면 된다  
	![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-4.png){: width="80%"}  

- 클래스 계층의 혼재와 클래스 계층의 분리
	- 클래스 계층 구조 하나에, '기능의 클래스 계층'과 '구현의 클래스 계층'이 혼재해 있으면, 새로운 하위 클래스를 만들 때 어려움이 있다
	- '기능의 클래스 계층'과 '구현의 클래스 계층'을 분리하고, 이들 사이에 '다리'를 놓는다  
	⇒ Bridge 패턴

## 02. 예제 프로그램

### '무언가를 표시하기'위한 프로그램  

|다리의 어느 쪽?|이름|해설|
|-----------------|----------------|-----------------|
|기능의 클래스 계층|Display|표시하는 클래스|
|기능의 클래스 계층|CountDisplay|지정 횟수만 표시하는 기능을 추가한 클래스|
|구현의 클래스 계층|DisplayImpl|표시하는 클래스|
|구현의 클래스 계층|StringDisplayImpl|문자열을 사용해서 표시하는 클래스|
||Main|동작 테스트용 클래스|

### 클래스 다이어그램  

![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-5.png){: width="100%"}

### 기능의 클래스 계층: Display 클래스
- 추상적인 '무언가를 표시하기 위한 것'으로 '기능의 클래스 계층'의 최상위에 존재
- impl 필드: Display 클래스의 '구현'을 나타내는 인스턴스
- 생성자
	- 구현을 나타내는 클래스(DisplayImpl)의 인스턴스를 인자로 넘겨 받는다
- open, print, close 메소드: 모두 DisplayImpl의 API를 호출한다
	- open(): 표시의 전 처리
	- print(): 표시하는 작업
	- close(): 표시 후의 처리
	- ⇒ Display의 API인 open, print, close가 DisplayImpl의 API인 rawOpen, rawPrint, rawClose 로 바뀌어 있음을 알 수 있다
- display()
	- open, print, close 를 차례로 호출한다  

```java
public class Display {
	private DisplayImpl impl;
	public Display(DisplayImpl impl) {
		this.impl = impl;
	}
	public void open() {
		impl.rawOpen();
	}
	public void print() {
		impl.rawPrint();
	}
	public void close() {
		impl.rawClose();
	}
	public final void display() {
		open();
		print();
		close();
	}
}
```

### 기능의 클래스 계층: CountDisplay 클래스
- Display 클래스에 기능을 추가함
	- Display 클래스에는 '표시한다'는 기능밖에 없음
	- CountDisplay에는 '지정횟수만큼 표시한다'라는 기능이 추가됨
		- mulitDisplay()  

```java
public class CountDisplay extends Display {
	public CountDisplay(DisplayImpl impl) {
		super(impl);
	}
	public void multiDisplay(int times) {
		open();
		for (int i = 0; i < times; i++) { // times회 반복해서 표시
			print();
		}
		close();
	}
}
```

### 구현의 클래스 계층: DisplayImpl 클래스
- '구현의 클래스 계층'의 최상위에 위치함
- 추상 클래스이며, rawOpen, rawPrint, rawClose 메소들르 가짐  

```java
public abstract class DisplayImpl {
	public abstract void rawOpen();
	public abstract void rawPrint();
	public abstract void rawClose();
}
```

### 구현의 클래스 계층: StringDisplayImpl 클래스
- 문자열을 표시하는 클래스
- <span style="color:blue">rawOpen</span>, rawPoint, <span style="color:blue">rawClose</span> 메소드 구현
	- printLine()를 이용해서 문자열을 표심함  

```java
public class StringDisplayImpl extends DisplayImpl {
	private String string;
	private int width;
	public StringDisplayImpl(String string) {
		this.string = string;
		this.width = string.getBytes().length;
	}
	public void rawOpen() {
		printLine();
	}
	public void rawPrint() {
		System.out.println("｜" + string + "｜");
	}
	private void rawClose() {
		printLine();
	}
	private void printLine() {
		System.out.print("+");
		for (int i = 0; i < width; i++) {
			System.out.print("-");
		}
		System.out.println("+");
	}
}
```

### Main 클래스
- Display나 CountDisplay 생성 시, 구현을 제공하는 클래스인 StringDisplayImpl의 객체를 인자로 넘겨준다  

```java
public class Main {
  public static void main(String[] args) {
    Display d1 = new Display(new StringDisplayImpl("Hello, Korea."));
    Display d2 = new CountDisplay(new StringDisplayImpl("Hello, World."));
    CountDisplay d3 = new CountDisplay(new StringDisplayImpl("Hello, Universe."));
    d1.display();
    d2.display();
    d3.display();
    d3.multiDisplay(5);
  }
}
```

![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-6.png){: width="80%"}

## 03. 등장 역할

- Abstraction의 역할
	- '기능의 클래스 계층'의 최상위에 있는 클래스
	- Implementor 역할의 메소드를 사용해서 기본적인 기능만을 제공하는 클래스
	- 예제에서 Display 클래스가 해당됨  

- RefinedAbstraction의 역할
	- Abstraction 역할에 기능을 추가한 역할
	- 예제에서 CountDisplay 클래스가 해당됨  

- Implementor의 역할
	- '구현의 클래스 계층'의 최상위에 있는 클래스
	- Abstraction 역할의 API를 구현하기 위한 메소드를 규정하는 역할
	- 예제에서 DisplayImpl 클래스가 해당됨  

- ConcreteImplementor의 역할
	- Implementor 역할의 API를 구체적으로 구현하는 역할
	- 예제에서, StringDisplayImpl 클래스가 해당됨  

- 클래스 다이어그램  

![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-7.png){: width="90%"}

## 04. 독자의 사고를 넓혀주는 힌트

### 분리해두면 확장이 편해진다
- 두 개로 클래스 계층을 나누어두면, 각각의 클래스 계층을 독립적으로 확장할 수 있다(예는 연습문제에서)
	- 기능을 추가하고 싶으면, 기능의 클래스 계층에 추가한다
		- 이 때 구현 클래스 계층은 전혀 수정할 필요가 없다  


	- 구현을 추가하고 싶으면 구현 클래스 계층을 확장한다
		- 예: 어떤 프로그램에 OS(운영체제) 의존 부분이 있어서, Window 판, Macintosh 판, Unix 판으로 구분된다고 하자 ⇒ OS에 의존하는 부분을 "구현의 클래스 계층"으로 표현한다
		- 각 OS 공통의 API를 정해서 Implementor 역할로 하고 Concrete Implementor 역할로 Window 판, Macintosh 판, Unix 판의 세 개의 클래스를 만든다
		- <span style="color:red">'기능의 클래스 계층'에 기능을 추가하더라도 각 OS에 대응 가능</span>
		- Display 생성시, 생성자에게 WindImpl, MacImpl, UnixImpl 중 적당한 것 하나를 넘겨주면 된다  
		![](https:///eliotjang.github.io/assets/images/system-analysis/ch09-8.png){: width="100%"}

### 상속은 견고한 연결, 위임은 느슨한 연결
- 상속은 소스 코드를 바꿀 수 없은 매우 견고한 연결임
	- 클래스간의 관계를 바꾸고 싶을 때는 상속을 사용해서는 안됨  

- Display 클래스 내에서 위임이 사용된다
	- 예: open을 실행할 때, <span style="color:red">impl.rawOpen()을 호출하여 '떠넘기기'를 한다</span>
		- 이 때, impl이 참조하고 있는 객체는, Display 생성 시 클라이언트(Main 클래스)가 넘겨준 구체적인 StringDisplayImpl 클래스의 인스턴스이다
		- StringDisplayImpl 이외의 다른 클래스의 객체를 넘겨주면, 구현이 교체되는 효과를 가져온다

## 05. 관련 패턴
- Template Method 패턴(3장)
- Abstract factory 패턴(8장)
- Adapter 패턴(2장)

## 06. 요약

- 두 종류의 클래스 계층 사이의 다리를 놓는 Bridge 패턴

- 구현의 클래스 계층에서 제공하는 메소드들을 이용하고 조합하여, 기능 클래스 계층에서 부가적인 기능을 제공한다


## 연습 문제

### 9-1
- '<u>임의 횟수만큼 문자열 찍기</u>' 기능을 하는 randomDisplay()를 제공하는 클래스 추가하기
    - 기능의 클래스 계층에 추가함
	- 기존의 StringDisplayImpl 클래스가 제공하는 메소드들을 이용해서 구현 가능하므로
- CountDisplay의 하위 클래스로, RandomCountDisplay 클래스를 정의함
- java.util.Random 클래스를 이용한다
	- random.nextInt(times)

### 9-2
- '<u>텍스트 파일의 내용을 표시</u>'처리를 하는 클래스를 예제 프로그램에 추가하기
	- 구현의 클래스 계층에 추가한다
		- 전혀 다른 방식의 DisplayImpl이 필요하므로
	- DisplayImpl의 하위 클래스로 FileDisplayImpl 클래스를 만듬
		- 제공하는 메소드 종류는 같다
			- rawOpen(), rawPrint(), rawClose()
- BufferedReader 이용
	- mark(int readAheadLimit): stream의 현재 위치에 표시를 해둠
		- readAheadLimit: 되돌릴 수 있는 최대 크기
			- 예: 4096이면, reset으로 되돌릴 수 있는 크기가 4096 바이트이다. 4096보다 더 많이 읽으면, reset 호출로 mark한 위치로 되돌아 갈 수 없다
		- reset(): 최근 표시 위치로 되돌림
			- rawPrint() 메소드 제일 앞 부분에서 최근 mark 된 위치로 되돌린다
		- mark() 함수로 위치를 기억시켜, reset() 함수로 mark() 함수로 기억한 곳으로 초기화

### 9-3
- '시작문자→장식문자 여러번→마지막 문자와 줄바꾸기'를 한 줄로해서, 이것이 여러 줄 반복된다
- 반복할 때 마다 점점 장식 문자의 개수가 증가된다
	- 하나씩 증가될 수도 있고, 둘씩, 또는 셋씩 증가될 수도 있다.
- '시작문자, 장식문자, 마지막문자 찍는 일'은 구현 클래스 계층에 추가하고
	- CharDisplayImpl 클래스
- '개수 증가하면서 여러 줄 반복해서 찍는 기능'은 기능 클래스 계층에 추가한다
	- IncreaseDisplay 클래스
- 추가한 구현은 다른 클래스(CountDisplay나 RandomCountDisplay)에서도 사용 가능함
- 추가한 다른 기능은, 다른 클래스 상에서도 작동한다






























