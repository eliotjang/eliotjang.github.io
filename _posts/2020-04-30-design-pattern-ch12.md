---
title: "[디자인 패턴] Chapter 11. Decorator 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch12. Decorator 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - 컴퓨터공학
tags:
  - 디자인 패턴
last_modified_at: 2020-04-30T21:00:00+09:00  
---  
# 장식과 내용물의 동일시

## 01. Decorator 패턴

![](https://eliotjang.github.io/assets/images/system-analysis/ch12-1.png){: width="40%"}

- 예: 케이크
  - 스펀지 케이스
  - 딸기를 얹으면 ⇒ 스트로베리 케이크
  - 화이트 초콜릿 + 초 ⇒ 생일 케이크

- 중심이 되는 객체에, 장식과 같은 부가적인 기능을 하나씩 입혀서 좀 더 목적에 어우리는 객체를 만든다

## 02. 예제 프로그램

### 문자열 주위에 장식(-,+,｜ 문자)을 입혀 표시하는 프로그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch12-2.png){: width="40%"}

|이름|해설|
|----------|---------------------------|
|Display|문자열 표시용 추상 클래스|
|StringDisplay|한 줄로 된 문자열 표시용 클래스|
|Border|'장식'을 나타내는 추상 클래스|
|SideBorder|좌우에만 장식이 붙은 클래스|
|FullBorder|상하좌우에 장식이 붙은 클래스|
|Main|동작 테스트용 클래스|

![](https://eliotjang.github.io/assets/images/system-analysis/ch12-3.png){: width="40%"}

### Display 클래스

- 여러 줄로 이루어진 문자열을 표시하기 위한 추상 클래스
- getColumns(): 가로의 문자 수를 얻기 위한 메소드
- getRows(): 세로의 줄의 수를 얻기 위한 메소드
- getRowText(): 지정한 행의 문자열을 얻기 위한 메소드
- show()
  - 모든 행을 화면에 표시하는 메소드
  - <span style="color:blue"><b>Template Method</b> 패턴이 적용됨</span>

```java
public abstract class Display {
  public abstract int getColumns(); // 가로 문자수 얻음
  public abstract int getRows(); // 세로 행수 얻음
  public abstract String getRowText(int row); // row번쨰 문자열 얻음
  public void show() {
    for (int i = 0; i < getRows(); i++) {
      System.out.println(getRowText(i));
    }
  }
}
```

### StringDisplay 클래스

- 한 줄의 문자열을 표시하는 클래스
- string 필드: 표시할 문자열을 저장함
- getColumns(): string.getBytes().length 를 이용하여 문자열이 차지하는 바이트 수를 반환함
- getRows(): 1을 반환함
- getRowText(int row): 입력 매개 변수 row가 0일 때만 string 필드를 반환함
- 이 클래스는, 여러 케이크의 중심에 있는 스펀지 케이크에 해당함

![](https://eliotjang.github.io/assets/images/system-analysis/ch12-4.png){: width="30%"}

```java
public class StringDisplay extends Display {
  private String string; // 표시 문자열
  public StringDisplay(String string) { // 인수로 표시할 문자열 지정
    this.string = string;
  }
  public int getColumns() { // 문자수
    return string.getBytes().length;
  }
  public int getRows() { // 행수는 1
    return 1;
  }
  public String getRowText(int row) { // row가 0일때만 반환
    if (row == 0) {
      return string;
    } else {
      return null;
    }
  }
}
```

### Border 클래스

- '장식'을 나타내는 추상 클래스
- Display의 하위 클래스로 정의됨
  - 상속에 의해 장식(Border)이 내용물(Display)과 동일한 메소드를 가진다
  - 장식과 내용물을 동일시 할 수 있다
  - 장식 클래스를 내용물로 해서 또 다른 장식을 붙일 수 있다는 의미
- display 필드: Display 형으로 선언됨
  - 장식이 감싸고 있는 "내용물"을 가리킨다
  - 이 필드는, StringDisplay 뿐만 아니라 Border 도 참조할 수 있다
    - 이유: Border도 Display의 하위 클래스 이므로

```java
public abstract class Border extends Display {
  protected Display display; // 이 장식을 둘러싸고 있는 '내용물'
  protected Border(Display display) { // 인스턴스 생성 시에 '내용물'을 인수로 지정
    this.display = display;
  }
}
```

### SideBorder 클래스

- Border의 하위 클래스
- 구체적인 장식의 일종
- 문자열 좌우에 정해진 문자(BorderChar)로 장식한다
- 생성자에서, 내용물(display)과 장식 문자(ch)가 지정됨
- getColumns(): 내용물의 문자 수에 2를 더한다
- getRows(): 내용물의 getRows()를 호출한다
- getRowText(): 내용물의 Text 양쪽에 장식 문자를 연결하여 반환한다

```java
public class SiderBorder extends Border {
  private char borderChar;
  public SlideBorder(Display display, char ch) {
    super(display);
    this.borderChar = ch;
  }
  public int getColumns() {
    return 1 + display.getColumns() + 1;
  }
  public int getRows() {
    return display.getRows();
  }
  public String getRowText(int row) {
    return borderChar + display.getRowText(rwo) + borderChar;
  }
}
```

### FullBorder 클래스

- Border의 하위 클래스
- 상하좌우에 장식을 한다
- SideBorder와 달리, 장식할 문자가 미리 고정되어 있다
- makeLine(char ch, int count): ch를 count 개수 만큼 연속해서 문자열로 만드는 메소드
- getRowText(int row)
  - 입력인자 row가 0 이거나, (내용물의 전체 줄 수 + 1)과 같으면 문자열 상단, 또는 하단에 장식할 문자열을 만든다

```java
public class FullBorder extends Border {
  public FullBorder(Display display) {
    super(display);
  }
  public int getColumns() {
    return 1 + display.getColumns() + 1;
  }
  public int getRows() {
    return 1 + display.getRows() + 1;
  }
  public String getRowText(int row) {
    if (row == 0) {
      return "+" + makeLine('-', display.getColumns()) + "+";
    } else if (row == display.getRows() + 1) {
      return "+" + makeLine('-', display.getColumns()) + "+";
    } else {
      return "｜" + display.getRowText(row - 1) + "｜";
    }
  }
  private Stirng makeLine(char ch, int count) {
    StringBuffer buf = new StringBuffer();
    for (int i = 0; i < count; i++) {
      buf.append(ch);
    }
    return buf.toString();
  }
}
```

### Main 클래스

- 동작 테스트용 클래스
- b1 객체: 'Hello, world.'를 장식하지 않고 표시한 것
- b2 객체: b1에 대해 '#' 문자로 좌우에 장식한 것
- b3 객체: b2에 대해 전체 장식을 한 것
- b4 객체: '안녕하세요'에 여러 겹 장식을 한 것

![](https://eliotjang.github.io/assets/images/system-analysis/ch12-5.png){: width="100%"}

```java
public class Main {
  public static void main(String[] args) {
    Display b1 = new StringDisplay("Hello, world.");
    Display b2 = new SideBorder(b1, '#');
    Display b3 = new FullBorder(b2);
    b1.show();
    b2.show();
    b3.show();
    Display b4 = new SideBorder(new FullBorder(new FullBorder(new SideBorder(new FullBorder(new StringDisplay("안녕하세요.")),'*'))),'/');
    b4.show();
```

### <span style="color:red">객체 다이어그램</span>

- b1의 장식이 b2, b2의 장식이 b3

![](https://eliotjang.github.io/assets/images/system-analysis/ch12-6.png){: width="80%"}

## 03. 등장 역할

- Component의 역할
  - 기능을 추가할 때 핵심이 되는 역할
  - 스펀지 케이크의 API에 해당함
  - 예제에서는, Display 클래스가 해당됨

- ConcreteComponent의 역할
  - Component 역할을 구현한 구체적인 클래스
  - 구체적인 스펀지 케이크에 해당함
  - 예제에서는, StringDisplay 클래스가 해당됨

- Decorator(장식자)의 역할
  - Component 역할과 동일한 인터페이스를 가짐
  - 장식자이면서, 장식할 대상이 되기도 함
  - 예제에서는, Border 클래스가 해당됨

- Concrete Decorator의 역할
  - 구체적인 장식자
  - 예제에서는, SideBorder와 FullBorder 클래스가 해당됨

![](https://eliotjang.github.io/assets/images/system-analysis/ch12-7.png){: width="80%"}

## 04. 독자의 사고를 넓혀주는 힌트

### 투명한 인터페이스(API)

- Border 클래스가 Display 클래스의 하위 클래스
  - 장식을 나타내는 Border 클래스가, 내용물을 나타내는 Display와 동일한 인터페이스(API)를 가진다  
  ⇒ 장식하는 클래스가, 다시 장식의 대상이 될 수 있다
  - getColumns, getRows, getRowText, show 메소드는 은폐되지 않고, 다른 클래스에서 볼 수 있다  
  ⇒ 투명함
  - Composite과 마찬가지로 재귀적인 구조임
    - 목적이 서로 다르다
      - <span style="color:red"><b>Composite</b> 패턴은, <b>container</b>가 다시 내용물이 될 수 있다</span>
      - <span style="color:blue"><b>Decorator</b></span> 패턴은, 장식하는 클래스가, 다시 장식 대상이 될 수 있다
      - 예: FullBorder의 getRowText()는 자신이 가지고 있는 Display 객체의 getRowText() 메소드를 호출한다

### 내용물을 변경하지 않고, 기능을 추가할 수 있다

- 내용물을 변경하지 않고, 새로운 장식을 계속해서 부착할 수 있다
- 포장되는 대상을 변경하지 않고, 기능을 추가할 수 있다
- Decorator 패턴에서는 위임이 사용된다
  - 예: SideBorder의 getColumns 메소드 내에서는, display.getColumns()를 호출한다

### 단순한 장식으로도 다양한 기능을 추가할 수 있다

- 간단한 구체적인 장식(ConcreteDecorator 역할)을 많이 준비해두고, 그것들을 자유롭게 조합하여 새로운 장식을 만들 수 있다

### java.io 패키지와 Decortator 패턴

![](https://eliotjang.github.io/assets/images/system-analysis/ch12-8.png){: width="80%"}

![](https://eliotjang.github.io/assets/images/system-analysis/ch12-9.png){: width="85%"}

### 작은 클래스가 증가함

- Decorator 패턴을 사용하면, 유사한 작은 클래스들이 많아지는 단점이 있다


## 05. 관련 패턴

- Adapter 패턴
- Strategy 패턴


## 06. 보강: 상속과 위임에 있어서의 동일시

![](https://eliotjang.github.io/assets/images/system-analysis/ch12-10.png){: width="80%"}

![](https://eliotjang.github.io/assets/images/system-analysis/ch12-11.png){: width="80%"}

![](https://eliotjang.github.io/assets/images/system-analysis/ch12-12.png){: width="80%"}

![](https://eliotjang.github.io/assets/images/system-analysis/ch12-13.png){: width="80%"}


## 07. 요약

- 객체를 차례차례 장식해서, 기능을 추가해 나가는 Decorator 패턴

## 연습 문제

- Q12-1
  - 문자열 아래 위로 장식문자를 추가하는 UpDownBorder클래스 만들기

- Q12-2
  - 여러 줄의 문자열을 표시하는 MultiStringDisplay 클래스 만들기
  - ConcreteComponent 역할 수행



















