---
title: "[시스템분석및설계] Chapter 17. Observer 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch17. Observer 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-05-31T23:55:00+09:00  
---  

## 01. Observer 패턴

- observer
  - 관찰자
  - 관찰 대상의 상태가 변하면, 관찰자에게 통지된다
  - 객체의 상태 변화에 따른 처리를 기술할 때 유용하게 사용된다

## 02. 예제 프로그램

- 수를 많이 생성하는 객체를 관찰자가 관찰해서, 그 값을 표시하는 프로그램
- 관찰자의 종류에 따라 표시 방법이 다르다
  - DigitObserver: 값을 숫자로 표시함
  - GraphObserver: 값을 간이 그래프로 표시함

|이름|해설|
|----|----|
|Observer|관찰자를 나타내는 인터페이스|
|NumberGenerator|수를 생성하는 객체를 나타내는 추상 클래스|
|RandomNumberGenerator|랜덤하게 수를 생성하는 클래스|
|DigitObserver|숫자로 수를 표시하는 클래스|
|GraphObserver|간이 그래프로 수를 표시하는 클래스|
|Main|동작 테스트용 클래스|  


### 클래스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch17-1.png){: width="100%"}  

### Observer 인터페이스

- 관찰자를 표현하는 인터페이스  
  - Java의 클래스 라이브러리에 있는 java.util.Observer와는 다르다
- update(NumberGenerator)
  - NumberGenerator가 "나의 내용이 갱신되었습니다. 표시도 갱신해주세요"라고 관찰자에게 알려줄 때 호출하는 메소드  

```java
public interface Observer {
  public abstract void update(NumberGenerator generator);
}
```

### NumberGenerator 클래스

- 수를 생성하는 추상 클래스
- execute(), getNumber(): 각각 수를 취득하고 생성하는 추상 메소드
- observers 필드: NumberGenerator를 관찰하고 있는 Observer들을 보관하고 있는 필드
- addObserver(Observer)
  - Observer를 추가할 때 호출하는 메소드
- deleteObserver(Observer)
  - Observer를 삭제할 때 호출하는 메소드
- notifyObservers()
  - Observer 전원에게, "나의 내용이 갱신되었기 때문에 당신의 표시도 갱신해주세요"라고 알려주는 메소드
  - Observer들의 update(this) 메소드를 차례차례 호출한다  

```java
import java.util.ArrayList;
import java.util.Iterator;

public abstract class NumberGenerator {
  private ArrayList observers = new ArrayList();
  public void addObserver(Observer observer) {
    observers.add(observer);
  }
  public void deleteObserver(Observer observer) {
    observers.remove(observer);
  }
  public void notifyObservers() {
    Iterator it = observers.iterator();
    while (it.hasNext()) {
      Observer o = (Observer)it.next();
      o.update(this);
    }
  }
  public abstract int getNumber();
  public abstract void execute();
}
```

### RandomNumberGenerator 클래스

- NumberGenerator의 하위 클래스
- 난수를 생성한다
  - java.util.Random 클래스 이용
- number 필드: 생성된 난수를 저장하는 변수
- getNumber(): number 필드의 값을 반환함
- execute():
  - 0~49까지의 난수 20개를 생성하고, 그 때마다 notifyObservers를 호출하여, 관찰자들에게 통지한다
  - Random 클래스의 nextInt() 이용  

```java
import java.util.Random;

public class RandomNumberGenerator extends NumberGenerator {
  private Random random = new Random();
  private int number;
  public int getNumber() {
    return number;
  }
  public void execute() {
    for (int i=0; i<20; i++) {
      number = random.nextInt(50);
      notifyObservers();
    }
  }
}
```

### DigitObserver 클래스

- Observer 인터페이스를 구현한 구체적인 관찰자
- 관찰한 수를 '숫자'로 표시
- update(NumberGenerator)
  - 인자로 전달된 NumberGenerator의 getNumber()를 이용하여 수를 얻어서 화면에 출력한다
  - 출력한 후, 표시된 모습을 잘 알 수 있도록 Thread.sleep()를 이용하여 속도를 늦춘다
    - Thread.sleep(100)
      - (100/1000 = 0.1초) 동안 CPU를 반환하고 쉬겠다는 뜻  

```java
public class DigitObserver implements Observer {
  public void update(NumberGenerator generator) {
    System.out.println("DigitObserver:" + generator.getNumber());
    try {
      Thread.sleep(100);
    } catch (InterruptedException e) {
    }
  }
}
```

### GraphObserver 클래스

- Observer 인터페이스를 구현한 구체적인 관찰자
- 관찰한 수를 '간단한 그래프'로 표시함
  - 관찰한 숫자만큼의 '*'를 출력  

```java
public class GraphObserver implements Observer {
  public void update(NumberGenerator generator) {
    System.out.print("GraphObserver:");
    int count = generator.getNumber();
    for (int i=0; i<count; i++) {
      System.out.print("*");
    }
    System.out.println("");
    try {
      Thread.sleep(100);
    } catch (InterruptedException e) {
    }
  }
}
```

### Main 클래스

- RandomNumberGenerator 인스턴스 1개 생성
- 관찰자 2개 생성
- RandomNumberGenerator에 관찰자 2개 등록
- RandomNumberGenerator의 execute()를 이용해서 수를 생성
  - 난수가 발생될 때마다, 관찰자들은 각자의 방식대로 수를 '표시'함  

```java
public class Main {
  public static void main(String[] args) {
    NumberGenerator generator = new RandomNumberGenerator();
    Observer observer1 = new DigitObserver();
    Observer observer2 = new GraphObserver();
    generator.addObserver(observer1);
    generator.addObserver(observer2);
    generator.execute();
  }
}
```

![](https://eliotjang.github.io/assets/images/system-analysis/ch17-2.png){: width="70%"}  


## 03. 등장 역할

- Subject(관찰 대상자) 역할
  - '관찰되는 쪽'을 나타냄
  - Observer 역할을 등록하는 메소드와 삭제하는 메소드를 가짐
  - 현재의 상태를 얻어갈 때 호출하는 메소드도 제공
  - 예제에서는, NumberGenerator 인터페이스가 해당  


- ConcreteSubject(구체적인 관찰대상자) 역할
  - 구체적인 '관찰되는 쪽'을 나타냄
  - 상태가 변하면, 등록된 Observer 역할에게 통보함
  - 예제에서는, RandomNumberGenerator 클래스가 해당됨  


- Observer(관찰자) 역할
  - '관찰하는 쪽'을 나타냄
  - Subject 역할로부터 상태변화를 통보받는 역할
  - 예제에서는, Observer 인터페이스가 해당됨
    - 통보받을 때, 관찰자의 update() 메소드가 호출됨  


- ConcreteObserver(구체적인 관찰자) 역할
  - 구체적인 '관찰하는 쪽'을 나타냄
  - 상태 변화가 관찰 대상으로부터 통보되면(즉, update 메소드가 호출되면) 그 메소드안에서 Subject 역할의 현재 상태를 얻어서 적당한 일을 수행
  - 예제에서는, DigitObserver와 GraphObserver 클래스가 해당됨  

![](https://eliotjang.github.io/assets/images/system-analysis/ch17-3.png){: width="100%"}  


## 04. 독자의 사고를 넓혀주는 힌트

- 여기서도 교환 가능성이 등장
  - RandomNumberGenerator 클래스는,
    - 현재 자신을 관찰하고 있는 것이 DigitObserver의 인스턴스인지 GraphObserver의 인스턴스인지 모름
    - 다만, observer 필드에 저장된 인스턴스들이 Observer 인터페이스를 구현하고 있다는 것만 암
  - 한편, DigitObserver 클래스는,
    - 자신이 관찰하고 있는 것이 RandomNumberGenerator의 인스턴스인지 다른 XXXXXNumberGenerator의 인스턴스인지 신경쓰지 않음
    - 다만, 관찰대상이 NumberGenerator의 하위 클래스의 인스턴스이고 getNumber 메소드를 가지고 있다는 것만 알고 있음
  - 관찰자와 관찰 대상을 논리적으로 분리함으로써, 각각을 쉽게 교체할 수 있음
  - ⇒ **확장성/교환 가능성이 높아짐**  



- 갱신을 위한 정보의 취급
  - NumberGenerator는 update 메소드를 사용해서 '갱신되었습니다'라고 Observer에게 통지함
    - 이때, Observer 클래스의 update()의 입력인자는 NumberGeneraoter
      - Observer는 입력인자로 들어온 NumberGenerator를 이용하여 원하는 정보를 추출한다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch17-4.png){: width="60%"}
      - NumberGenerator가, 관찰자가 필요한 정보만 넘겨줄 수도 있다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch17-5.png){: width="60%"}  



- '관찰'하는 것이 아니고, 사실은 '통지'를 받는다
  - Observer의 역할이, 사실은 Subject로부터 통지가 오기를 기다리는, 수동적인 역할을 하고 있다
  - 그래서, Observer 패턴을 Publish-Subscribe 패턴이라고도 한다  



- Model/View/Controller(MVC)
  - Smalltalk 언어에서, 하나의 데이터 모델을 여러 형태로 보여주고자 할 때 사용되는 유명한 패턴이다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch17-6.png){: width="70%"}
  - Model과 View의 관계는, Observer 패턴에서 Subject와 Observer의 역할과 서로 대응된다


## 05. 보강

- JDK에서 제공되는 java.util.Observer 인터페이스와 java.util.Observable 클래스는, Observer 패턴의 일종이다
- java.util.Observer 인터페이스  
![](https://eliotjang.github.io/assets/images/system-analysis/ch17-7.png){: width="70%"}
- java.util.Observer와 java.util.Observerble을 사용하기가 쉽지 않다
  - 관찰대상이 되는 클래스를 정의하기 위해서는 java.util.Observable 클래스를 상속받아야 한다
  - 그런데, 이미 Subject 역할로 생각하고 있는 클래스가 다른 클래스의 하위 클래스이면,
  - Observable 클래스를 또 상속받을 수 가 없다
    - 자바는 다중상속을 지원하지 않으므로
  - 따라서, 사용하기가 쉽지 않다


## 06. 관련 패턴

- Mediator 패턴


## 07. 요약

- 객체의 상태변화를 다른 객체에게 통지하는 Observer 패턴


## 연습 문제

- 17-1
  - 예제 프로그램에, 관찰대상으로서의 IncrementalNumberGenerator 클래스를 추가하기
    - 시작하는 수와 마지막 수 및 증가분을 입력받는 생성자를 가진다
    - 예: new IncrementalNumberGenerator(10, 50, 5) 인 경우,
      - 10부터 50까지 5씩 증가하면서 숫자를 생성
    - 숫자가 만들어질 때 마다 관찰자에게 통지한다  


- 17-2
  - 예제 프로그램에 관찰자 3개를 추가하기
    - FrameObserver
      - BorderLayout 매니저: 프레임 영역을 동서남북중앙으로 나눔
      - update(): GraphText와 GraphCanvas의 update()를 호출
    - GraphText
      - 숫자만큼의 '*'를 출력하는 관찰자
    - GraphCanvas
      - Graphics 객체의 fillArc(int x, int y, int width, int height, int startAngle, int arcAngle)
      - startAngle부터 archAngle 만큼 그린다
      - startAngle: 0 이면 3시 위치를 의미한다(반시계방향으로 증가)
      - arcAngle: 양수이면 반시계 방향으로 그리고, 음수이면 시계방향으로 그린다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch17-8.png){: width="50%"}  


## Homework #4: Observer 패턴 응용

- 예제 프로그램에 관찰자 및 관찰 대상 추가하기
  - 관찰자 클래스: NamePrintObserver
    - 관찰 대상자로부터 통지가 올 때 마다, 관찰 대상자로부터 숫자를 얻어 그 숫자만큼의 자기 이름을 새로운 한 줄에 계속 출력한다
    - 예: 관찰대상자로부터 얻은 숫자가 5인 경우
      - 출력: 장성원 장성원 장성원 장성원 장성원  


  - 관찰대상 클래스: PrimeNumberGenerator
    - NumberGenerator의 하위 클래스로 둔다
    - 1부터 50 사이의 소수(prime number)를 생성한다
    - 생성할 때 마다 등록된 관찰자들에게 통지한다  


  - Main 클래스의 main()
    - PrimeNumberGenerator의 인스턴스인 png를 생성한다
    - DigitObserver와 GraphObserver 이외에 NamePrintObserver의 인스턴스를 생성한다
    - png에 위의 3가지 Observer를 등록한다
    - png.execute()를 실행한다






































