---
title: "[디자인 패턴] Chapter 0. UML 과 Design Pattern"  
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch0. UML과 Design Pattern"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg

categories: 
  - 컴퓨터공학
tags:
  - design pattern

last_modified_at: 2020-03-17T19:00:00+09:00  
---  

## 01. UML에 대해서
### UML(Unified Modeling Language)  
  - 시스템을 시각화하거나 사양이나 설계를 문서화 하기 위한 표현 방법
  - 본 교재에서는, 클래스나 인터페이스의 관계를 표현하기 위해 사용
  - Resource  
    <https://www.omg.org/uml>  
    <https://www.rational.com/uml>  

### 클래스 다이어그램  
  - 클래스나 인스턴스(객체), 인터페이스 등 간의 정적인 관계를 표현한 그림  

![](https://eliotjang.github.io/assets/images/system-analysis/ch00-1.png){: align-center}  

클래스 다이어그램 예제 설명  
  - ParentClass : 상위 클래스, 부모 클래스
  - ChildClass : 하위 클래스, 파생 클래스, 자식 클래스. 

부가적인 정보  
  - 추상 클래스(abstract class)  
    - 추상 메소드를 하나 이상 가지고 있는 클래스
    - 이탤릭체로 이름을 표현. 
  - 정적 필드(static field)  
    - 객체가 아니라 클래스에 포함된 변수(객체마다 하나씩 생기지 않는다)
    - 밑줄을 그어 이름을 표현.  
  - 추상 메소드(abstract method)  
    - 구현 부분이 없는 메소드 --> 자식 클래스가 구현해야 한다.  
    - 이탤릭체로 이름을 표현. 
  - 정적 메소드(static method)  
    - 객체가 아니라 클래스에 포함된 메소드(정적 메소드나 정적 필드만을 접근할 수 있다) 
    - 밑줄을 그어 이름을 표현한다.  

### 인터페이스와 구현
  - 인터페이스란, 구현 부분이 생략되어 있는 메소드들의 이름만 선언되어 있다.
  - 설명 : PrintClass 클래스가 Printable 인터페이스를 구현한다.  

![](https://eliotjang.github.io/assets/images/system-analysis/ch00-2.png){: align-center}  

### 집합(Aggregation)  
  - '갖고 있는 관계'는 aggregation으로 표현한다.  
  - 설명:  
    - Basket 클래스는 Fruit 클래스의 인스턴스를 가지고 있다. 즉, Basket 클래스에는, Fruit 클래스로 선언된 필드(변수)가 있다.  
    - Fruit 클래스도 Color 클래스의 인스턴스를 가진다.  
![](https://eliotjang.github.io/assets/images/system-analysis/ch00-3.png){: align-center}  

### 액세스 제어
  - public 메소드나 필드 : +
  - private 메소드나 필드 : -
  - protected 메소드나 필드 : #  

![](https://eliotjang.github.io/assets/images/system-analysis/ch00-4.png){: align-center}  

### 클래스간의 관게
  - 클래스간의 관계를 나타내기 위해서 클래스를 연결하고, 그 위에 관계를 나타내는 이름을 붙인다.  
  - 삼각형 방향으로 해석한다.  

![](https://eliotjang.github.io/assets/images/system-analysis/ch00-5.png){: align-center}  

### 시퀀스(sequence) 다이어그램  

![](https://eliotjang.github.io/assets/images/system-analysis/ch00-6.png){: align-center}  
  - 프로그램이 동작할 때, 객체들 사이의 메소드들이 어떤 순서로 실행되는지를 보여준다.  
    - 객체들간의 협동 과정을 보여줌
  - `:Client` : Client 클래스의 익명의 객체를 의미한다.  
  - `clientA:Client` : Client 클래스의 clientA라는 이름의 객체를 의미한다. 
  - 사각형 아래의 점선은 생명선(life line)을 나타낸다. 
    - 인스턴스(객체)가 존재하는 기간을 의미함
  - 생명선 중간의 가늘고 긴 사각형: 객체가 활동 중임을 나타낸다. 
    - 즉, CPU를 얻어서 실행이 되는 상태  

## 02. 디자인 패턴을 배우기 전에  

디자인 패턴은 클래스 라이브러리가 아니다  
  - 클래스 라이브러리 : 많이 사용되는 클래스들을 미리 만들어서 모아둔 것  
    `예: 통신 관련 클래스 라이브러리, 수학 관련 클래스 라이브러리`  

클래스 라이브러리 구현 시, 디자인 패턴이 적용된다.  
  - `예: Java.util.Calender 클래스에의 getInstance() 메소드에서 Factory Method 패턴이 사용된다.`  

다이어그램을 단순히 보지 말고, 그 의미를 읽어내야 한다.  

스스로 예제를 생각해야 한다.  


