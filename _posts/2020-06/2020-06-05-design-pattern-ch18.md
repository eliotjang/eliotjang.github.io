---
title: "[디자인 패턴] Chapter 18. Memento 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch18. Memento 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - 컴퓨터공학
tags:
  - 디자인-패턴
  - java

last_modified_at: 2020-06-05T19:00:00+09:00  
---  

## 01. Memento 패턴  

- undo 기능을 제공하려면 이전의 상태를 누군가가 기억하고 있어야 한다.  

- Memento 패턴 사용 목적  
  - undo(실행 취소)
  - redo(재실행)
  - history(작업 이력의 작성)
  - snapshot(현재 상태의 저장)

- memento의 뜻
  - 기념물, 유품, 추억거리  

## 02. 예제 프로그램  

- 주사위 게임으로 과일 모으기  
  - 게임 규칙  
    - 이 게임은 자동으로 진행된다.  
    - 게이머가 주사위를 던져 나온 수가 다음 상태를 결정한다.  
    - 좋은 수가 나오면 게이머의 돈이 증가한다.  
    - 나쁜 수가 나오면 돈이 줄어든다.  
    - 특별히 좋은 수가 나오면 게이머는 과일을 받는다.  
    - 돈이 다 없어지면 종료한다.  

- 돈이 많이 모이면, 장래를 위해 Memento 클래스의 인스턴스를 만들어 '현재의 상태(돈과 과일)'를 보존한다  

- 계속해서 돈이 줄어들어 돈이 반 미만으로 줄면, Memento의 인스턴스를 사용해서 보존해 두었던 이전 상태로 복귀한다  


## 02. 예제 프로그램  

|패키지|이름|해설|
|------|----|----|
|game|Memento|Gamer의 상태를 나타내는 클래스|
|game|Gamer|게임을 하는 주인공의 클래스<br/>Memento의 인스턴스를 만든다|
|Anonymous|Main|게임을 진행시키는 클래스<br/>Memento의 인스턴스를 보존해 두고 필요에 따라서 Gamer의 상태를 복원한다|  


![](https://eliotjang.github.io/assets/images/system-analysis/ch18-1.png){: width="100%"}  


### Memento 클래스  

- 게이머의 상태를 표현하는 클래스  
- money 필드: 현재 소유한 돈  
- fruits 필드: 현재 가지고 있는 과일  
- 생성자에 public이 없다 ⇒ package 접근 권한  
  - 같은 패키지에 속하는 클래스에서만 사용할 수 있다  
- addFruit(String)
  - 과일을 추가할 때 호출하는 메소드  
  - package 접근 권한 ⇒ game 패키지 외부에서는 이 클래스의 내부 상태를 변경할 수 없다  
- getMoney()  

```java
package game;
import java.util.*;

public class Memento {
  int money;
  ArrayList fruits;
  public int getMoney() {
    return money;
  }
  Memento(int money) {
    this.money = money;
    this.fruits = new ArrayList();
  }
  void addFruit(String fruit) { 
    fruits.add(fruit);
  }
  List getFruits() {
    return (List)fruits.clone();
  }
}
```

### Gamer 클래스  

- 게임을 수행하는 게이머를 표현하는 클래스  
- 돈과 과일, 난수 발생기, 과일 이름 배열을 가진다  
- bet()
  - 1이 나오면, 돈이 100원 증가한다  
  - 2가 나오면, 돈이 반으로 준다  
  - 6이 나오면, 과일을 받는다  
- createMemento()
  - 현재 상태를 보존하는 Memento 객체를 생성함
  - 현재 돈과 맛있는 과일들 만을 Memento 객체에 보존한다 
  - 찰칵하고 사진을 찍듯이 현재의 상태를 Memento 인스턴스에 보존해두는 것이다 
    - startsWith(): 문자열 인스턴스의 시작 부분과 지정한 문자열이 일치하는지를 확인 
- restoreMemento()
  - undo를 행하는 메소드 
  - Memento 객체로부터 보존되었던 돈과 과일을 얻어온다 
  - Memento 인스턴스를 기초로 자신의 상태를 복원 
- getFruit()
  - 선택적으로 '맛있는'을 과일이름 앞에 붙인다  


```java
package game;
import java.util.*;

public class Gamer {
  private int money;
  private List fruits = new ArrayList();
  private Random random = new Random();
  private static String[] fruitsname = {
    "사과", "포도", "바나나", "귤",
  };
  public Gamer(int money) {
    this.money = money;
  }
  public int getMoney() {
    return money;
  }
  public void bet() {
    int dice = random.nextInt(6) + 1;
    if (dice == 1) {
      money += 100;
      System.out.println("소지금이 증가했습니다.");
    } else if (dice == 2) {
      money /= 2;
      System.out.println("소지금이 절반이 되었습니다.");
    } else if (dice == 6) {
      String f = getFruit();
      System.out.println("과일(" + f + ")을 받았습니다.");
      fruits.add(f);
    } else {
      System.out.println("변한 것이 없습니다.");
    }
  }
  public Memento createMemento() {
    Memento m = new Memento(money);
    Iterator it = fruits.iterator();
    while (it.hasNext()) {
      String f = (String)it.next();
      if (f.startsWith("맛있는 ")) {
        m.addFruit(f);
      }
    }
    return m;
  }
  public void restoreMemento(Memento memento) {
    this.money = memento.money;
    this.fruits = memento.getFruits();
  }
  public String toString() {
    return "[money = " + money + ", fruits = " + fruits + "]";
  }
  private String getFruit() {
    String prefix = "";
    if (random.nextBoolean()) {
      prefix = "맛있는 ";
    }
    return prefix + fruitsname[random.nextInt(fruitsname.length)];
  }
}
```  

### Main 클래스 

- gamer.createMemento()를 호출하여 현재 gamer의 상태를 보존한다 
- 현재 게이머의 돈이 Memento 객체의 돈보다 크면,
  - 현재 상태를 Memento 객체에 보존한다 
- 현재 게이머의 돈이 Memento 객체의 돈의 반보다 작으면,
  - Memento 객체에 보존된 이전 상태로 복귀한다  

```java
import game.Memento;
import game.Gamer;

public class Main {
  public static void main(String[] args) {
    Gamer gamer = new Gamer(100);
    Memento memento = gamer.createMemento();
    for (int i = 0; i < 100; i++) {
      System.out.println("==== " + i);
      System.out.println("현상:" + gamer);

      gamer.bet();

      System.out.println("소지금은" + gamer.getMoney() + "원이 되었습니다.");

      if (gamer.getMoney() > memento.getMoney()) {
        System.out.println("    (많이 증가했으므로 현재의 상태를 저장하자)");
        memento = gamer.createMemento();
      } else if (gamer.getMoney() < memento.getMoney() / 2) { 
        System.out.println("    (많이 감소했으므로 이전의 상태로 복원하자)");
        gamer.restoreMemento(memento);
      }

      try {
        Thread.sleep(1000);
      } catch (InterruptedException e) {
      }
      System.out.println("");
    }
  }
}
```  

![](https://eliotjang.github.io/assets/images/system-analysis/ch18-2.png){: width="80%"}  


![](https://eliotjang.github.io/assets/images/system-analysis/ch18-3.png){: width="40%"}  

![](https://eliotjang.github.io/assets/images/system-analysis/ch18-4.png){: width="40%"}  

## 03. 등장 역할 

- Originator(작성자)의 역할 
  - Memento 역할을 만들어 자신의 현재 상태를 보존시키는 역할을 한다 
  - 또한, Memento로부터 이전 상태를 받아서 복귀하는 일을 한다 
  - 예제에서는, Gamer 클래스가 해당됨 

- Memento(기념품)의 역할
  - Origiantor 역할의 내부 정보를 보존한다 
  - 예제에서는 Memento 클래스가 해당됨 

- Caretaker(돌보는 사람)의 역할 
  - Origiantor 역할의 상태를 보존하고 싶을 때 Origiantor에게 알리는 역할을 담당한다 
  - 예제에서는, Main 클래스가 해당됨 

- Originator 역할과 Memento 역할은 견고하게 결속되어 있다 
- Caretaker 역할과 Memento 역할은 느슨하게 연결되어 있다 
  - Caretaker 역할이 Memento 내부의 정보를 쉽게 접근할 수 없다 

![](https://eliotjang.github.io/assets/images/system-analysis/ch18-5.png){: width="100%"}  

## 04. 독자의 사고를 넓혀주는 힌트  

- 액세스 제어  

  - |액세스 제어|해설|
|-----------|----|
|public|모든 클래스에서 보인다|
|protected|그 패키지 및 하위 클래스에서 보인다|
|없음|그 패키지에서만 보인다|
|private|그 클래스에서만 보인다|  

  - Memento 클래스의 생성자는 아무것도 안 붙어 있음 
    - Main 클래스는 Memento의 생성자를 호출할 수 없다 
    - Main 클래스 안에서 Memento 객체를 생성할 수 없다 
    - Memento가 필요할 때 마다, Gamer 클래스의 createMemento를 호출한다 

- Memento를 몇 개 가질까?
  - 예제에서는, Main 클래스가 가지고 있는 Memento는 하나뿐이었다 
  - 배열 등을 사용하면 Main이 여러 개의 Memento 인스턴스를 생성해서 가지고 있을 수 있다 
    - 즉, 여러 시점의 상태를 보존해 둘 수 있다 

- Caretaker 역할과 Originator 역할을 분리하는 의미 
  - "undo를 하고 싶으면, Originator 역할에게 그 기능을 만들어 넣으면 되지, 일부러 디자인 패턴을 사용할 필요가 있을까?" 
  - Caretaker의 역할 
    - 어느 시점에서 스냅샷을 찍어, 언제 undo를 실행할 지 결정함 
    - Memento 역할을 보관하는 일을 함 
  - Originator의 역할 
    - Memento 역할을 생성하는 일을 함 
    - Memento 역학을 사용해서 자신의 상태를 원래대로 되돌리는 일을 함 
  - 분리해 두면 좋은 점 
    - 다음과 같이 수정을 할 때, Originator 역할은 변경할 필요가 없다 
      - 여러 단계의 undo를 실행하도록 변경하고 싶을 때
      - undo만 하는 것이 아니라, 현재의 상태를 파일에 보존하고 싶을 때 


## 05. 관련 패턴 

- Command 패턴(22장)
- Prototype 패턴(6장) 
- State 패턴 (19장) 


## 06. 요약 

- 현재 객체의 상태를 기록하여 보존하기 위한 Memento 패턴 
  - Caretaker 역할은 중간 중간 Originator 역할에게 부탁하여 '현재의 상태'를 표현하는 Memento 역할을 만든다 
  - Caretaker 역할은 필요할 때 서랍 안에서 Memento 역할을 꺼내서 Origiantor 역할에게 건네주면, 그 상태로 복원이 된다 


## 연습 문제 

- 18-2
  - Memento의 getMoney()만 public으로 선언되어 있어, 외부에서 이 메소드를 통해서만 내부 정보를 접근할 수 있다 
  - Memento의 다른 메소드들도 public으로 선언되어, Caretaker 역할이 Memento 역할을 자유롭게 조작할 수 있다면 어떤 불편함이 있을까?
- 18-2
  - Memento가 보존해야 할 정보가 대량인 경우, 이에 대처하는 방법은?
- 18-3
  - Memento 클래스의 변수 number의 액세스 제어 문제 
- 18-4
  - Memento의 인스턴스를 파일로 보존하는 방법 
    - Memento 클래스가 Serializable 인터페이스를 구현하도록 한다 
      - Serializable 인터페이스를 구현한 클래스의 객체는, ObjectOutputStream의 writeObject()나 ObjectInputStream의 readObject() 메소드를 통해서 파일에 쓰고/읽을 수 있다 
  - Memento의 내용을 파일에 쓸 때, 압축 기능을 이용하는 방법 
    - 1) `import.java.util.zip.*;` 를 추가한다
    - 2) 파일에 쓸 때 다음 코드로 수정한다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch18-6.png){: width="80%"}
    - 3) 파일로부터 읽을 때 다음 코드로 수정한다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch18-7.png){: width="80%"}






























