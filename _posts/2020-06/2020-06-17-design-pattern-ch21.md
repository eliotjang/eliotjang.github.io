---
title: "[디자인 패턴] Chapter 21. Proxy 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch21. Proxy 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - 컴퓨터공학
tags:
  - 디자인-패턴
  - java

last_modified_at: 2020-06-17T20:00:00+09:00  
---  

## 01. Proxy 패턴  

- Proxy
  - 대리인 
  - 본인을 대신해서 일을 처리하는 사람  



## 02. 예제 프로그램  

- 이름을 가진 프린터 
  - 화면에 문자열을 표시하는 프로그램 
  - 
|이름|해설|
|----|----|
|Printer|이름이 붙은 프린터를 나타내는 클래스(본인)|
|Printable|Printer와 PrinterProxy의 공통 인터페이스|
|PrinterProxy|이름이 붙은 프린터를 나타내는 클래스(대리인)|
|Main|동작 테스트용 클래스|  


![](https://eliotjang.github.io/assets/images/system-analysis/ch21-1.png){: width="100%"}  


### Printable 인터페이스  

- PrinterProxy와 Printer 클래스를 동일시하기 위한 인터페이스  


```java
public interface Printable {
  public abstract void setPrinterName(String name);
  public abstract String getPrinterName();
  public abstract void print(String string);
}
```  


### Printer 클래스  

- '본인'을 나타내는 클래스 
- 생성자 : heavyJob(5초가 걸리는 상대적으로 오래 걸리는 일)을 호출함 
  - Printer 클래스의 인스턴스 생성에 많은 시간이 걸린다는 것을 전제로 
- setPrinterName() / getPrinterName()
  - 프린터 이름의 설정과 취득 
    - 예제에서 프린터 이름은 Alice와 Bob
- print()
  - 프린터 이름과 문자열을 화면에 출력함 
- heavyJob()
  - Thread.sleep(1000) 후 '.'을 찍는 일을 5번 실행함  


```java
public class Printer implements Printable {
  private String name;
  public Printer() {
    heavyJob("Printer의 인스턴스를 생성 중");
  }
  public Printer(String name) {
    this.name = name;
    heavyJob("Printer의 인스턴스를 생성 중");
  }
  public void setPrinterName(String name) {
    this.name = name;
  }
  public String getPrinterName() {
    return name;
  }
  public void print(String string) {
    System.out.println("=== " + name + " ===");
    System.out.println(string);
  }
  private void heavyJob(String msg) {
    System.out.print(msg);
    for (int i = 0; i < 5; i++) {
      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
      }
      System.out.print(".");
    }
    System.out.println("완료");
  }
}
```  


### PrinterProxy 클래스  

- Printer 클래스의 '대리인'을 나타내는 클래스 
- name 필드 : 대리인의 이름을 저장함 
- real 필드 : '본인'에 대한 참조를 저장함 
- setPrintername() : real이 null이 아니면, 즉, 본인 객체가 이미 생성되어 있으면, 그 객체에도 이름을 설정한다 
- print()
  - realize() 호출 후, '본인' 객체의 print() 메소드를 호출한다  
⇒ real 필드에는 본인(Printer 클래스의 인스턴스 저장)이 저장되어 있기 때문에 real.print를 호출  
⇒ 위임  
- realize()
  - 실제 일을 하는 '본인' 객체가 생성되지 않았으면, 생성한다 
- setPrintername()과 realize()는 synchronized로 선언됨  


```java
public class PrinterProxy implements Printable {
  private String name;
  private Printer real;
  public PrinterProxy() {
  }
  public PrinterProxy(String name) {
    this.name = name;
  }
  public synchronized void setPrinterName(String name) {
    if (real != null) {
      real.setPrinterName(name);
    }
    this.name = name;
  }
  public String getPrinterName() {
    return name;
  }
  public void print(String string) {
    realize();
    real.print(string);
  }
  private synchronized void realize() {
    if (real == null) {
      real = new Printer(name);
    }
  }
}
```  


### Main 클래스  

- PrinterProxy를 경유해서, Printer를 이용하는 클래스 
- 먼저, PrinterProxy 생성 후, 이름을 설정한 후, "Hello, world"를 출력한다 
- 이름을 설정하거나 표시하는 동안에는, Printer 클래스의 인스턴스(본인)는 아직 생성되지 않는다 
- print() 메소드가 호출되어야 비로소, Printer 객체가 생성된다  


```java
public class Main {
  public static void main(String[] args) {
    Printable p = new PrinterProxy("Alice");
    System.out.println("이름은 현재" + p.getPrinterName() + "입니다.");
    p.setPrinterName("Bob");
    System.out.println("이름은 현재" + p.getPrinterName() + "입니다.");
    p.print("Hello, world");
  }
}
```  

![](https://eliotjang.github.io/assets/images/system-analysis/ch21-2.png){: width="80%"}  


### 시퀀스 다이어그램  

![](https://eliotjang.github.io/assets/images/system-analysis/ch21-3.png){: width="80%"}  



## 03. 등장 역할  

- Subject(주체)의 역할 
  - Proxy와 RealSubject를 동일시하기 위한 인터페이스(API)를 정한다 
  - 예제에서는, Printable 인터페이스가 해당됨 
- Proxy(대리인)의 역할 
  - Client로부터의 요구를 가능한 한 처리함 
  - 혼자서 처리할 수 없으면 RealSubject 역할에게 위임한다 
  - Proxy 역할은, RealSubject 역할이 정말로 필요해지면 그 때가서 RealSubject 역할을 생성한다 
  - Subject 역할에서 정해져 있는 인터페이스(API)를 구현 
  - 예제에서는 PrinterProxy 클래스가 해당됨  
- RealSubject(실제의 주체)의 역할 
  - '대리인'인 Proxy 역할에서 감당할 수 없는 일이 발생했을 등장하는 것이 '본인'인 RealSubject 역할 
  - Subject 역할에서 정해져 있는 인터페이스(API)를 구현 
  - 예제에서는, Printer 클래스가 해당됨  
- Client(의뢰자)의 역할 
  - Proxy 패턴을 이용하는 역할 
  - 예제에서는, Main 클래스가 해당됨  

![](https://eliotjang.github.io/assets/images/system-analysis/ch21-4.png){: width="90%"}  


## 04. 독자의 사고를 넓혀주는 힌트  

- 대리인을 사용해서 스피드 업 
  - Proxy 역할이 대리인이 되어서, 할 수 있는 일은 모두 처리한다 
  - 실제로 무거운 처리(예: Printer 인스턴스 생성)는 필요할 때까지 지연시킨다 
  - 초기화하는데 시간이 많이 걸리는 대규모의 시스템에서 유용하다 
    - 기동하는 시점에서 아직 이용하지 않는 기능까지 전부 초기화하면, 애플리케이션 기동에 시간이 걸림 
    - 시간이 걸리는 아직 필요하지 않는 초기화는, 필요한 시점가지 미루자  
- 대리인과 본인을 분리할 필요가 있는가?
  - PrinterProxy 클래스와 Printer 클래스로 나누지 않고, Printer 클래스 안에 처음부터 지연 평가(late evaluation) 기능(필요하게되면 비로소 인스턴스를 생성하는 기능)을 넣어 둘 수도 있다 
  - 그러나, Proxy 역할과 RealSubject 역할을 분리함으로써 프로그램의 부품화가 가능함 
    - 개별적으로 수정이 가능하다 
    - 무엇을 대리인이 처리하고, 무엇을 본인이 처리할 지 변경할 수 있다 
    - 지연 평가를 실행하고 싶지 않다면, 클라이언트(Main)에서 Printer 클래스의 인스턴스를 new 하면 된다 
- 대리와 위임 
  - 대리인이 처리할 수 있는 일은 대리인이 처리한다 
  - 대리인이 처리할 수 없는 일은 본인에게 위임한다 
- 투명적(투과적)이라는것(transparent)
  - 클라이언트(Main) 입장에서는, 실제로 호출하는 것이 PrinterProxy 객체인지, Printer 객체인지 상관하지 않는다(투명하게 보인다) 
- HTTP Proxy
  - Proxy는 HTTP 서버(웹 서버)와 HTTP 클라이언트(웹 브라우저) 사이에 들어가, 웹 페이지의 캐싱 등을 실행하는 소프트웨어
  - 여기에도, Proxy 패턴을 적용할 수 있다 
    - 웹 브라우저가 웹 페이지를 표시할 때, 일일이 원격지에 있는 웹서버로부터 페이지를 얻어오는 것이 아니다 
    - 대신에, HTTP 프록시가 캐쉬한 페이지를 얻어온다 
    - 최신 정보가 필요하게 되었거나, 페이지의 유효기간이 다 되었을 때에 비로소 웹 페이지를 가지러 웹 서버로 간다 
    - 웹 브라우저 ⇒ Client 역할 
    - HTTP Proxy ⇒ Proxy 역할 
    - 웹 서버 ⇒ RealSubject 역할 
- 다양한 Proxy
  - Virtual Proxy(가상 프록시)
    - 이번 장 예제와 같이, 인스턴스가 필요해진 시점에서 생성, 초기화를 실행하는 객체 
  - Remote Proxy(원격 프록시)
    - RealSubject 역할이 멀리 떨어져 있는데도, 클라이언트는 이 원격 프록시를 통해서 자신의 근처에 있는 것처럼 메소드를 호출할 수 있다. Javadml RMI가 여기에 해당 
  - Access Proxy
    - RealSubject 역할의 기능에 대해, 접근 제한 기능을 수행하는 프록시. 정해진 사용자이면 호출을 허락하지만, 그 외에는 에러로 처리  


## 05. 관련 패턴  

- Adapter 패턴
- Decorator 패턴  


## 06. 요약  

- 본인이 필요해질 때까지 대리인에게 일을 맡기는 Proxy 패턴  


## 연습 문제  

- 21-1
  - PrinterProxy 클래스가 Printer 클래스를 '몰라도 상관없도록' PrinterProxy 클래스를 수정 
- 21-2
  - PrinterProxy 클래스에서는 setPrinterName 메소드와 realize 메소드가 synchronized 메소드로 되어 있다 
  - synchronized 메소드로 하지 않았을 경우에 발생하는 문제  


















