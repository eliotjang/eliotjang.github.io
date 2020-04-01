---
title: "[시스템분석및설계] Chapter 2. 어댑터(Adapter) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-04-01T01:00:00+09:00  
---  

## 01. Adapter 패턴  

**AC 어댑터**  
  - 기존의 교류 100볼트 전기를, 직류 12볼트로 바꾸어 준다  

  ![](https://eliotjang.github.io/assets/images/system-analysis/ch02-1.png){: width="60%" height="45%"}  


**Adapter 패턴**  
  - 이미 제공되어 있는 것을 그대로 사용할 수 없는 경우
  - '이미 제공되어 있는 것'과 '필요한 것' 사이의 간격을 메우는 디자인 패턴
  - Wrapper 패턴이라고도 한다.  

**두 가지 종류의 Adpater 패턴**  
  - 상속(inheritance)을 이용한 Adapter 패턴
  - 위임(delegation)을 이용한 Adapter 패턴  


## 02. 예제 프로그램 1 - 상속을 이용한 것  

**Banner 클래스**
  - showWithParen(): 문자열 앞뒤에 괄호를 쳐서 표시하는 메소드
  - showWithAster(): 문자열 앞뒤에 '*'를 붙여서 표시하는 메소드
  - 이 두 메소드를 '이미 제공되어 있는 것'으로 가정한다.  

**Print 인터페이스**
  - printWeak(): 문자열을 약하게 표시(괄호를 붙임)
  - printStrong(): 문자열을 강하게 표시('*' 붙임)
  - 이 두 메소드를 직류 12볼트와 같은 '필요한 것'이라고 가정한다.  

**목표**
  - Banner 클래스라는 기존의 클래스를 이용해서, Print 인터페이스를 충족시키는(즉, 구현하는) 클래스를 만들고자 한다.  


```java
public class Banner {
  private String string;
  public Banner(String string) {
    this.string = string;
  }
  public void showWithParen() {
    System.out.println("(" + string + ")");
  }
  public void showWithAster() {
    System.out.println("*" + string + "*");
  }
}

public interface Print {
  public abstract void printWeak();
  public abstract void printStrong();
}
```

**이미 제공되는 것:   ** ![](https://eliotjang.github.io/assets/images/system-analysis/ch02-2.png){: width="20%" height="25%"}  

**이미 필요로 하는 것:** ![](https://eliotjang.github.io/assets/images/system-analysis/ch02-3.png){: width="20%" height="25%"}  

필요한 메소드는, Print 클래스의 printWeak와 printStrong이다.  
그러나, 같은 일을 하는 메소드를 Banner 클래스가 제공한다.  
⇒ Banner 클래스의 메소드들을 재활용하는 Adapter 클래스를 만든다.  
⇒ PrintBanner 클래스  

**PrintBanner 클래스**
  - Banner 클래스를 <font color="blue">상속</font>하면서, Print 인터페이스를 <font color="blue">구현</font>한다.  

```java
public class PrintBanner extends Banner implements Print {
  public PrintBanner(String string) {
    super(string);
  }
  public void printWeak() {
    showWithParen();
  }
  public void printStrong() {
    showWithAster();
  }
}
```  


**전원 예와 예제 프로그램**  

||전원의 비유|예제 프로그램|
|-----------|--------|----------------------|
|제공되어 있는 것|교류 100볼트|Banner클래스(showWithParen, showWithAste()|
|교환장치|어댑터|PrintBanner클래스|
|필요한 것|직류 12볼트|Print 인터페이스(printWeak, printStrong)|  


**Banner 클래스 소스 (sample1/Banner.java)**  

**Print 인터페이스 소스 (sample1/Print.java)**  

**PrintBanner 클래스 소스 (sample1/PrintBanner.java)**
  - Banner 클래스를 <font color="red">상속</font>, Print 인터페이스를 <font color="red">구현</font>
  - printWeak(): 상속받은 showWithParen()을 호출한다.
  - printStrong(): 상속받은 showWithAster()를 호출한다.  

**클래스 다이어그램**  

![](https://eliotjang.github.io/assets/images/system-analysis/ch02-4.png){: width="660" height="300"}  

**Main 클래스 소스 (sample1/Main.java)**
  - PrintBanner의 인스턴스를 Print 인터페이스 형의 변수에 할당
      - Main 클래스는, Print 인터페이스를 사용해서 프로그래밍했음
      - 실제 일을 수행하는 Banner 클래스의 메소드를, Main 클래스에서는 완전히 은폐되어 있다.
  - <font color="red">Main 클래스를 수정하지 않고도, PrintBanner 클래스의 구현을 변경할 수 있다.</font>  

```java
public class Main {
  public static void main(String[] args) {
    Print p = new PrintBanner("Hello");
    p.printWeak();
    p.printStrong();
  }
}
```  


## 03. 에제 프로그램 2 - 위임을 이용한 것  


**위임**
  - 내가 할 일을 누군가에게 맡긴다.
  - 예제에서 PrintBanner는 자신이 할 일을 Banner 클래스의 인스턴스에게 맡긴다.  
  
**예제1과 달리, Print를 클래스로 가정하면,**
  - PrintBanner가 Print와 Banner의 하위 클래스로 정의할 수 없다.(다중 상속을 지원하지 않기 때문에)
  - 위임을 사용해야 한다.  

**Print 클래스 (sample2/Print.java)**
  - 추상 클래스로 정의됨  

**PrintBanner 클래스 (sample2/PrintBanner.java)**
  - banner 필드가 Banner 클래스의 인스턴스를 가진다.
  - printWeak():
      - banner의 showWithParen()을 <font color="blue">호출</font>한다.
      - PrintBanner 자신이 일을 처리하지 않고, Banner에게 <font color="red">위임</font>한다.
  - printStrong():
      - banner의 showWithAster()을 <font color="blue">호출</font>한다.
      - PrintBanner 자신이 일을 처리하지 않고, Banner에게 <font color="red">위임</font>한다.  

```java
public abstract class Print {
  public abstract void printWeak();
  public abstract void printStrong();
}

public class PrintBanner extends Print {
  private Banner banner;
  public PrintBanner(String string) {
    this.banner = new Banner(string);
  }
  public void printWeak() {
    banner.showWithParen();
  }
  public void printStrong() {
    banner.showWithAster();
  }
}
```  

**클래스 다이어그램**  

![](https://eliotjang.github.io/assets/images/system-analysis/ch02-5.png){: width="650" height="400"}  


## 04. Adapter 패턴에 등장하는 역할  

**Target(대상)의 역할**
  - 지금 필요한 메소드를 결정하는 역할
  - 에졔: 직류 12볼트 또는 Print 인터페이스나 클래스  

**Client(의뢰자)의 역할**
  - Target 역할의 메소드를 이용하는 역할
  - 예제: 12볼트로 동작하는 노트북이나 Main 클래스  

**Adaptee(개조되는 측)의 역할**
  - 이미 준비되어 있는 메소드를 제공하는 역할
  - 예제: 교류 100볼트 전원이나 Banner 클래스  

**Adapter의 역할**
  - Target 역할을 실제로 만족시키는 역할
  - 에제: 어댑터나 PrintBanner 클래스  

**상속을 이용한 패턴**  

![](https://eliotjang.github.io/assets/images/system-analysis/ch02-6.png){: width="650" height="400"}  


**위임을 이용한 패턴**  

![](https://eliotjang.github.io/assets/images/system-analysis/ch02-7.png){: width="650" height="400"}  


## 05. 독자의 사고를 넓혀주는 힌트  

**어떤 경우에 사용할까**  
  - 기존의 클래스를 개조해서 필요한 클래스로 만들 때 사용함
  - 기존의 클래스가 충분히 테스트되어 있을 때 더욱 좋다.
      - 재사용  

**만약 소스가 없더라도**
  - 기존의 클래스는 손을 대지 않고, 새로운 인터페이스에 기존의 클래스를 맞출 수 있다.
  - 특히, 기존 클래스의 소스 코드 없이, 메소드의 프로토타입만 알면 어댑터 패턴을 적용할 수 있다.
      - 메소드 프로토타입: 메소드의 이름, 인자 개수, 인자형, 반환형 등  

**버전업과 호환성**
  - 소프트웨어를 버전업 할 때 중요한 문제는, '기존의 버전과의 호환성'이다.
  - 새로운 버전을 Adaptee,, 기존 버전을 Target 역할을 하도록 하면, 기존의 버전과 새로운 버전을 공존시킬 수 있다.
  - 예: methodA1()을 methodA2()로 버전업 하는 경우  

  ![](https://eliotjang.github.io/assets/images/system-analysis/ch02-8.png){: width="750" height="360"}  


## 06. Adapter 패턴과 관련된 패턴  

**Bridge 패턴(9장)**  
**Decorator 패턴(12장)**  


## 07. 이 장에서 학습한 내용  

**인터페이스(API)가 다른 두 개의 사이에 들어가 그 사이를 메우는 Adapter 패턴**
  - API: Application Programming Interfaces  


## 연습문제  

**2-2**
  - 기존의 클래스: java.util.Properties  ![](https://eliotjang.github.io/assets/images/system-analysis/ch02-9.png){: width="106" height="104"}
  - 어댑터 FileProperties: Properties 상속, FileIO 구현
      - Main 클래스는, FileIO의 인터페이스를 호출한다.
      - FileIO를 구현한 FileProperties의 각 메소드들은 Properties의 해당 메소드를 호출한다.  
      ![](https://eliotjang.github.io/assets/images/system-analysis/ch02-10.png){: width="280" height="110"}  

























