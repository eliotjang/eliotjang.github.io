---
title: "[시스템분석및설계] Chapter 3.  템플릿(Template) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-04-01T12:00:00+09:00  
---  

## 01. Template Method 패턴  

**템플릿인란?**
  - 문자 모양을 따라 구멍에 뚫려있는 얇은 플라스틱 판
  - 필기도구의 종류(싸인펜, 연필, 색연필)에 따라 실제로 쓰여지는 문자의 인스턴스가 결정된다.  

  ![](https://eliotjang.github.io/assets/images/system-analysis/ch03-1.png){: width="500" height="190"}  

**Template Method 패턴**
  - 상위 클래스에, 템플릿 역할을 하는 메소드가 정의됨.
      - 그 메소드에서는 추상 메소드들을 호출한다.
      - 상위 클래스의 이 메소드만 보면, 추상 메소드들이 어떻게 호출되는지는 알 수 있다. 하지만, 최종적으로 어떤 처리를 하는지는 모른다.
  - 하위 클래스가 추상 메소드를 실제로 구현한다.
      - 다른 하위 클래스가 다른 구현을 하면, 다른 처리가 실행된다.
      - 하위 클래스에서 어떤 구현을 하더라도, 처리의 큰 흐름은 상위 클래스가 결정한대로 이루어진다.
  - 상위 클래스에서 처리의 흐름을 결정하고, 하위 클래스에서 구체적인 내용을 결정하는 디자인 패턴  

**클래스 다이어그램**  

![](https://eliotjang.github.io/assets/images/system-analysis/ch03-2.png){: width="350" height="300"}  

  - AbstractDisplay
      - display() 메소드: 이 안에서 open, print, close 메소드가 사용(호출)된다.
      - open, print, close는 추상 메소드임
      - <font color="blue">display()</font> 메소드를 <font color="red">Template Method</font>라고 한다.
  - open, print close 메소드를 실제로 구현하고 있는 것은, CharDisplay와 StringDisplay 클래스이다.  

|이름|해설|
|------------------|-------------------------------------|
|AbstractDisplay|Display 메소드만 구현되어 있는 추상클래스|
|CharDisplay|open, print, close 메소드를 구현하고 있는 클래스|
|StringDisplay|open, print, close 메소드를 구현하고 있는 클래스|
|Main|동작 테스트용 클래스|  

> 해당 클래스를 <font color="blue"><b>클릭</b></font>하여 기능과 코드를 살펴볼 수 있습니다.  
<details>
<summary><font color="blue><b>AbstractDisplay 클래스</b></font></summary>
<div markdown="1">

**AbstractDisplay 클래스 (sample/AbstractDisplay.java)**
  - display()
      - open 메소드를 호출한다.
      - print 메소드를 5번 호출한다.
      - close 메소드를 호출한다.
  - open, print, close 추상 메소드가 실제로 하는 일은, 하위 클래스를 살펴보아야 한다.  

```java
public abstract class AbstractDisplay { // 추상 클래스 AbstractDisplay
  public abstract void open(); // 하위 클래스에 구현을 맡기는 추상 메소드(1) open
  public abstract void print(); // 하위 클래스에 구현을 맡기는 추상 메소드(2) print
  public abstract void close(); // 하위 클래스에 구현을 맡기는 추상 메소드 (3) close
  public final void display() { // 추상 클래스에서 구현되고 있는 메소드 display
    open(); // 우선 open하고,
    for (int i=0; i < 5; i++) { // 5번 print를 반복하고,
      print();
    }
    close(); // 마지막으로 close한다. 이것이 display 메소드에서 구현되고 있는 내용.
  }
}
```  

</div>
</details>  

<details>
<summary><font color="blue><b>CharDisplay 클래스<b/></font></summary>
<div markdown="1">

**CharDisplay 클래스 (sample/CharDisplay.java)**
  - 상위 클래스인 AbstractDisplay 클래스의 추상 메소드인 open, print, close를 구현함  
  |메소드명|처리|
  |------|----------------------------------|
  |open|문자열 '<<'를 표시합니다.|
  |print|생성자에서 제공된 문자 한 개를 표시합니다.|
  |close|문자열 '>>'를 표시합니다.|  

```java
public class CharDisplay extends AbstractDisplay { // CharDisplay는 AbstractDisplay의 하위클래스
  private char ch; // 표시해야 할 문자
  public CharDisplay(char ch) { // 생성자에서 전달된 문자 ch를
    this.ch = ch; // 필드에 기억해 둔다.
  }
  public void open() { // 상위 클래스에서는 추상 메소드였다. 여기에서 오버라이드해서 구현
    System.out.print("<<"); // 개시 문자열 "<<"을 표시한다.
  }
  public void print() { // print 메소드도 여기에서 구현한다. 이것이 display에서 반복해서 호출된다.
    System.out.print(ch); // 필드에 기억해 둔 문자를 1개 표시한다.
  }
  public void close() { // close 메소드도 여기에서 구현.
    System.out.println(">>"); // 종료 문자열 ">>"을 표시.
  }
}
```  

</div>
</details>

<details>
<summary><font color="blue><b>StringDisplay 클래스</b></font></summary>
<div markdown="1">

**StringDisplay 클래스 (sample/StringDisplay.java)**
  - 상위 클래스인 AbstractDisplay 클래스의 추상 메소드인 open, print, close를 구현함
  - string.getBytes().length 문장
      - 문자열의 바이트 개수를 얻는다.
      - string.length()와 어떻게 다른가?
	  - string = "안녕하세요."인 경우
	      - string.length() : 6을 반환
	      - string.getBytes().length() : 13 을 반환  
      > <font color="green">getBytes() 메소드 호출 시, 아무 매개변수도 넘겨주지 않으면 현재 플랫폼의 기본 캐릭터 셋으로 디코딩하여 바이트 배열을 생성</font>  
      > <font color="green">캐릭터 셋을 지정하여 getBytes() 메소드를 사용하면 지정된 캐릭터 셋으로 해당 문자열을 디코딩하여 바이트 배열 생성  
  |메소드명|처리|
  |--------|----------------------------------------------------|
  |open|문자열 '+----+'를 표시합니다.|
  |print|생성자에서 제공된 문자열을 '|'와 '|' 사이에 두어 표시합니다.|
  |close|문자열 '+----+'를 표시합니다.|  

```java
public class StringDisplay extends AbstractDisplay { // StringDisplay도 AbstractDisplay의 하위 클래스.
  private String string; // 표시해야 할 문자열.
  private int width; // 바이트 단위로 계산한 문자열의 「폭」.
  public StringDisplay(String string) { // 생성자에서 전달된 문자열 string을
    this.string = string; // 필드에 기억.
    this.width = string.getBytes().length; // 그리고 바이트 단위의 폭도 필드에 기억해두고 나중에 사용한다.
  }
  public void print() { // print 메소드는
    System.out.println("|" + string + "|"); // 필드에 기억해 둔 문자열의 전후에 "|"을 붙여서 표시.
  }
  public void close() { // close 메소드는
    printLine(); // open 메소드처럼 printLine 메소드에서 선을 그리고 있다.
  }
  private void printLine() { // open과 close에서 호출된 printLine 메소드이다. private이기 때문에 이 클래스 안에서만 사용된다.
    System.out.print("+"); // 테두리의 모서리를 표현하는 "+" 마크를 표시.
    for (int i=0; i < width; i++) { // width개의 "-"를 표시하고
      System.out.print("-"); // 테두리 선으로 이용한다.
    }
    System.out.println("+"); // 테두리의 모서리를 표현하는 "+" 마크를 표시.
  }
}  

</div>
</details>

<details>
<summary><font color="blue><b>Main 클래스</b></font></summary>
<div markdown="1">

**Main 클래스 (sample/Main.java)**
  - CharDisplay와 StringDisplay의 인스턴스를 생성한 후, display()를 호출한다.
  - d1.display() / d2.display() / d3.display()의 실제 동작은, d1, d2, d3 객체가 속한 클래스에서 결정한다.  

```java
public class Main {
  public static void main(String[] args) {
    // 'H'를 가진 CharDisplay 인스턴스를 1개 만든다.
    AbstractDisplay d1 = new CharDisplay("H");
    // "Hello, world."를 가진 StringDisplay의 인스턴스를 1개 만든다.
    AbstractDisplay d2 = new StringDisplay("Hello, world.");
    // "안녕하세요."를 가진 StringDisplay의 인스턴스를 1개 만든다.
    AbstractDisplay d3 = new StringDisplay("안녕하세요.");
    d1.display(); // d1, d2, d3 모두 AbstractDisplay의 하위클래스의 인스턴스이기 때문에
    d2.display(); // 상속한 display 메소드를 호출할 수 있다.
    d3.display(); // 실제 동작은 CharDisplay나 StringDisplay에서 결정한다.
  }
}
```  

![](https://eliotjang.github.io/assets/images/system-analysis/ch03-3.png){: width="310" height="260"}  

</div>
</details>


## 03. Template Method 패턴에 등장하는 역할  

**AbstractClass(추상 클래스)의 역할**
  - 템플릿 메소드 구현
  - 템플릿 메소드에서 사용하는 추상 메소드 선언
  - 예제에서, AbstractDisplay 클래스가 해당됨  

**ConcreteClass(구체적 클래스)의 역할**
  - AbstractClass 역할에서 선언되어 있는 추상 메소드를 구현
  - 에제에서, CharDisplay와 StringDisplay 클래스가 해당됨  

**일반적인 클래스 다이어 그램**  

![](https://eliotjang.github.io/assets/images/system-analysis/ch03-4.png){: width="140" height="310"}  

**로직을 공통화할 수 있다**
  - TemplateMethod 패턴을 이용하지 않은 경우  
  ![](https://eliotjang.github.io/assets/images/system-analysis/ch03-5.png){: width="380" height="270"}  

      - templateMethod()가 method1,2,3을 같은 방식(로직)으로 사용하는 경우, 같은 코드가 ConcreteClass 1,2,3에 반복해서 나타난다.  
      ⇒ 로직을 바꾸어야 하는 경우, Concrete1,2,3의 모든 templateMethod()를 수정해야 한다.  
  - TemplateMethod 패턴을 이용한 경우  
  ![](https://eliotjang.github.io/assets/images/system-analysis/ch03-6.png){: width="350" height="300"}  

      - templateMethod()가 부모 클래스인 AbstractClass에서 구현됨으로써, <font color="red">공통된 로직이 한 곳에 집중</font>되어 있다.  


## 05. 관련 패턴  

**Factory 패턴(4장)**  
**Stratedy 패턴(10장)**  


## 06. 보강: 클래스 계층과 추상 클래스  

**상위 클래스가 하위 클래스에게 요청**
  - 상위 클래스가 선언된 추상 클래스의 구현을, 하위 클래스에게 요청한다.  

**추상 클래스의 의의**
  - 추상 클래스는 인스턴스를 만들 수 없다.
  - 메소드의 이름은 추상 클래스인 상위 클래스에서 결정하고, 실제 하는 일은 하위 클래스에서 결정한다.  

**<font color="red">리스코프의 치환 원칙(LSP)</font> <font color="green">(상속의 일반원칙)</font>**
  - <font color="red">소프트웨어공학 교재 p.255 참조</font>
      - 상위 클래스형의 변수에 하위 클래스의 어떠한 인스턴스를 대입해도 작동  
      ```java
      Abstract d1 = new CharDisplay("H");
      Abstract d2 = new StringDisplay("Hello, world");
      ```  


## 07. 요점  

**상위 클래스에서 처리의 흐름을 규정하고,**  
**하위 클래스에서 처리의 내용을 구체화하는 패턴**  


## 연습문제 (각자 공부할 것)  

**3-1**
  - java.io.InputStream 클래스에서 사용되는 TemplateMethod 패턴  

**3-2**
  - final 메소드의 의미  

**3-3**
  - 메소드 accessibility 문제  

**3-4**
  - 추상 클래스와 인터페이스 클래스의 차이점
  - <font color="red">참고: Java 8의 디폰트 메소드 공부할 것!!!</font>  




