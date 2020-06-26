---
title: "[시스템분석및설계] Chapter 14. Chain of Responsibility 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch14. Chain of Responsibility 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-05-19T23:00:00+09:00  
---  

## 01. Chain of Responsibility 패턴

- "책임 떠넘기기"가 필요한 경우가 있다
  - ex) 어떤 요구가 발생했을 때 그 요구를 처리할 객체를 바로 결정할 수 없는 경우에는 다수의 객체를 사슬처럼 연결해 두고 객체의 사슬을 차례로 돌아다니면서 목적에 맞는 객체를 결정하는 경우

- Chain of Responsibility
  - 어떤 사람에게 요구가 들어온다 ➔ 그 사람이 그것을 처리할 수 있으면 처리하고, 처리할 수 없으면 '다음 사람'에게 넘긴다


## 02. 예제 프로그램

- trouble이 발생해서, 누군가가 처리해야 하는 상황

|이름|해설|
|----|----|
|Trouble|발생한 트러블을 나타내는 클래스. 트러블 번호(number)를 가짐|
|Support|트러블을 해결하는 추상 클래스|
|NoSupport|트러블을 해결하는 구상 클래스(항상 '처리하지 않음')
|LimitSupport|트러블을 해결하는 구상 클래스(지정한 번호 미만의 트러블을 해결)|
|OddSupport|트러블을 해결하는 구상 클래스(홀수 번호의 트러블을 해결)|
|SpecialSupport|트러블을 해결하는 구상 클래스(지정 번호의 트러블을 해결)|
|Main|Support들의 사슬을 만들어 트러블을 일으키는 동작 테스트용 클래스|

- 클래스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch14-1.png){: width="100%" }

### Trouble 클래스
- number 필드: 트러블 번호를 유지함
- getNumber()
  - 트러블 번호를 반환하는 메소드

```java
public class Trouble {
  private int number; // 트러블 번호
  public Trouble(int number) { // 트러블 번호 생성
    this.number = number;
  }
  public int getNumber() {
    return number;
  }
  public String toString() {
    return "[Trouble " + number + "]";
  }
}
```

### Support 클래스

- 트러블을 해결할 사슬을 만들기 위한 추상 클래스
- name 필드: 트러블 해결자의 이름
- next 필드:
  - 자기 다음 번에 트러블을 해결할 객체(떠넘기기를 할 곳)를 가리킨다
- setNext()
  - next 필드를 설정한다
- support()
  - <span style="color:blue"><b>추상 메소드 resolve() 메소드</b></span>를 호출해서, 반환 값이 false 이면, 자기 다음 해결자에게 트러블을 떠넘긴다
  - Template Method 패턴을 사용했다

```java
public abstract class Support {
  private String name; // 이 트러블 해결자의 이름
  private Support next; // 떠넘기는 곳
  public Support(String name) { // 트러블 해결자의 생성
    this.name = name;
  }
  public Support setNext(Support next) { // 떠넘기는 곳 설정
    this.next = next;
    return next;
  }
  public final void support(Trouble trouble) { // 트러블 해결의 수순
    if (resolve(trouble)) {
      done(trouble);
    } else if (next != null) {
      next.support(trouble);
    } else {
      fail(trouble);
    }
  }
  public String toString() {  // 문자열 표현
    return "[" + name + "]";
  }
  protected abstract boolean resolve(Trouble trouble); // 해결용 메소드
  protected void done(Trouble trouble) {
    System.out.println(trouble + "is resolved by" + this + ".");
  }
  protected void fail(Trouble trouble) {
    System.out.println(trouble + "cannot be resolved.");
  }
}
```

### NoSupport 클래스

- Suport의 하위 클래스
- resolve()
  - 항상 false를 반환
  - 즉, 어떠한 트러블도 해결하지 않음

```java
public class NoSupport extends Support {
  public NoSupport(String name) {
    super(name);
  }
  protected boolean resolve(Trouble trouble) {
    return false;
  }
}
```

### LimitSupport 클래스

- Support의 하위 클래스
- limit의 지정한 번호 미만의 트러블 만을 해결하는 클래스

```java
public class LimitSupport extends Support {
  private int limit;
  public LimitSupport(String name, int limit) {
    super(name);
    this.limit = limit;
  }
  protected boolean resolve(Trouble trouble) {
    if (trouble.getNumber() < limit) {
      return true;
    } else {
      return false;
    }
  }
}
```

### OddSupport 클래스

- 홀수 번호의 트러블을 처리하는 클래스

```java
public class OddSupport extends Support {
  public OddSupport(String name) {
    super(name);
  }
  protected boolean resolve(Trouble trouble) {
    if (trouble.getNumber() % 2 == 1) {
      return true;
    } else {
      return false;
    }
  }
}
```

### SpecialSupport 클래스

- 지정한 번호의 트러블에 한해서 처리하는 클래스

```java
public class SpecialSupport extends Support {
  private int number;
  public SpecialSupport(String name, int number) {
    super(name);
    this.number = number;
  }
  protected boolean resolve(Trouble trouble) {
    if (trouble.getNumber() == number) {
      return true;
    } else {
      return false;
    }
  }
}
```

### Main 클래스

- 알고리즘
  - 여섯 명의 트러블 해결자 생성
  - Chain of Responsibility 형성
  - 다양한 트러블 발생시켜, Alice에게 전달
- 결과
  - 처음에는 Bob이 분발한다
  - Bob이 해결할 수 없을 때 Diana가 등장
  - Alice는 전혀 등장하지 않는다(모든 트러블을 떠넘기므로)
  - 429번 트러블은, Charlie가 해결한다

```java
public class Main {
  public static void main(String[] args) {
    Support alice = new NoSupport("Alice");
    Support bob = new LimitSupport("Bob", 100);
    Support charlie = new SpecialSupport("Charlie", 429);
    Support diana = new LimitSupport("Diana", 200);
    Support elmo = new OddSupport("Elmo");
    Support fred = new LimitSupport("Fred", 300);
    // 사슬 형성
    alice.setNext(bob).setNext(charlie).setNext(diana).setNext(elmo).setNext(fred);
    // 트러블 생성
    for (int i=0; i<500; i+=33) {
      alice.support(new Trouble(i));
    }
  }
}
```

![](https://eliotjang.github.io/assets/images/system-analysis/ch14-2.png){: width="70%" }

- 363번 트러블을 처리할 때의 시퀀스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch14-3.png){: width="100%" }


## 03. 등장 역할

- 클래스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch14-4.png){: width="100%" }


## 04. 독자의 사고를 넓혀주는 힌트

- 요구하는 사람과 요구를 처리하는 사람을 유연하게 연결
  - Client는 맨 처음 사람에게만 요구를 한다
  - 그 요구가 처리자들을 연결한 사슬을 돌아다니다가
  - 적절한 처리자에 의해 처리된다
  - 이 패턴을 사용하지 않는다면, "이 요구는 이 처리자가 처리해야 한다"라는 지식을 누군가 중앙집권적으로 가지고 있어야 한다
    - 이 지식을 요구자에게 맡기는 것은 현명하지 않다

- 동적으로 사슬의 형태를 바꿀 수 있다
  - Alice ~ Fred 까지의 서포트 팀이 항상 고정되는 것은 아니다
  - 동적으로 처리자들의 순서를 바꿀 수 있다

- 자신의 일에 집중할 수 있다
  - 자신이 할 수 없으면 재빨리 '다음 사람'에게 넘김으로써, 각각의 처리자가 자신의 일에만 집중할 수 있다

- 떠넘기기로 처리가 늦어지지 않을까?
  - 유연성은 높지만, 처리 속도는 느리다
  - 요구와 처리자의 관계가 고정적이고, 처리의 속도가 중요한 경우에는 이 패턴을 사용하지 않는 편이 유효할 수 있다


## 05. 관련 패턴

- Composite 패턴
- Command 패턴


## 06. 요약

- 요구를 처리하는 객체들을 사슬 모양으로 늘어놓고, 요청이 들어오면 누가 처리할 지를 차례차례 체크해 나가는 Chian of Responsibility 패턴


## 연습 문제

- 14-1
  - 윈도우 시스템에서, Chain of Responsibility 패턴이 자주 사용된다
  - 윈도우 시스템의 버튼, 텍스트박스, 체크 박스 등에 마우스 클릭 이벤트가 발생하면?

![](https://eliotjang.github.io/assets/images/system-analysis/ch14-5.png){: width="70%" }

- 14-2
  - 윈도우 시스템의 또 다른 Chain of Responsibility 패턴 사용 사례
  - 그림 14-5의 작은 다이얼로그
  - '폰트' 리스트 박스에 포커스가 있을 때 화살표 키를 움직이면 리스트 박스 안의 선택되는 폰트가 변함
  - 체크 박스에 포커스가 있을 때 ↑키를 누르면 포커스가 '폰트'리스트로 이동해서 ↓키를 눌러도 체크 박스로 포커스가 돌아오지 않음
  - Chain of Responsibility 패턴 적용

- 14-3
  - Support 클래스의 support 메소드는 public 으로 선언된 반면,
  - resolve 메소드는 protected로 선언된 의도는 무엇일까?

- 14-4
  - 예제 프로그램의 Support 클래스의 support 메소드를, 재귀적으로 호출하지 말고, for-loop로 전개하시오

![](https://eliotjang.github.io/assets/images/system-analysis/ch14-6.png){: width="80%" }


## Homework 2: Chain of Responsibility 패턴 응용

- 14장 에제 프로그램에, HwSupport 클래스 추가하기
  - 기능: 트러블 번호가 소수(prime number)이면 자신이 처리하고, 소수가 아니면 다음 처리자에게 넘긴다
  - "소수 구하는 메소드 calcPrimeNumber(int)"를 HwSupport에 작성할 것
  - Main의 main()에서는
    - '자신의 이름'을 객체 참조 변수로 하여, HwSupport 객체를 생성한다
      - 예: Support seongwon = new HwSupport("장성원");
    - 이 객체를, 책임 사슬의 맨 앞에 둔다
    - 나머지는 예제 프로그램과 그대로 둔다

- 숙제 제출 방법
  - __보고서__
    - 표지 (과목명, 학번, 이름, 제출일, 담당교수 등)
    - 코드 설명 (표를 이용하는 등 보기 좋게 작성)
      - 각 클래스가 하는 일
      - 각 클래스의 각 메소드가 하는 일
    - 코드 프린트
  - __프로그램__
    - 해람인의 e참뜰 과제물 제출 기능을 이용
      - 바이러스 체크 후 압축파일로 제출
      - 각 클래스의 소스 파일 (java)
      - 모든 클래스 컴파일 한 파일 (class)
