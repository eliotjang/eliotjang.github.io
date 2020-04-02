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

![](https://eliotjang.github.io/assets/images/system-analysis/ch03-1.png){: width="350" height="300"}  

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

> 해당 클래스를 <font color="blue">클릭</font>하여 기능과 코드를 살펴볼 수 있습니다.  
<details>
<summary><font color="blue>AbstractDisplay 클래스</font></summary>
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
<summary><font color="blue>CharDisplay 클래스</font></summary>
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

<details>
<summary><font color="blue>StringDisplay 클래스</font></summary>
<div markdown="1">

**StringDisplay 클래스 (sample

</div>

<details>
<summary><font color="blue>Main 클래스</font></summary>
<div markdown="1">



</div>























































