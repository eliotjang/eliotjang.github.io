---
title: "[시스템분석및설계] Chapter 16. Mediator 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch16. Mediator 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-05-23T15:00:00+09:00  
---  

## 01. Mediator 패턴

- 멤버 10명이 서로 공동 작업을 하는데, 서로 각자 의견 교환과 지시를 해서 대혼란이 발생한다
  - 중간에 의견을 조정하는 사람을 두면 좋겠다

- Mediator 패턴
  - '조정자', '중개자'라는 뜻
  - '중개인'이라고 생각하면 좋다
    - 모든 멤버(colleague)는 '중개인'에게 보고한다
    - '중개인'은 각 멤버가 올린 보고를 토대로 대국적인 지시를 내린다
    - 모든 멤버는 이 지시에 따른다

## 02. 예제 프로그램

- GUI 애플리케이션
  - '이름과 패스워드를 입력하는 로그인 다이얼로그'
  - Guest/Login 선택과 여러 상황에 따라
    - Username과 Password 입력란의 활성화 여부
    - OK/Cancel 버튼 활성화 여부

![](https://eliotjang.github.io/assets/images/system-analysis/ch16-1.png){: width="30%" }  

![](https://eliotjang.github.io/assets/images/system-analysis/ch16-2.png){: width="100%" }

- 이러한 프로그램을 어떻게 만들 것인가?
  - 라디오 버튼, 텍스트 필드, 버튼은 각각 다른 클래스로 되어 있다
  - 앞에서의 로직을 각 클래스에 분산시키면 코딩하기가 힘들어진다
    - 각각의 객체가 서로 관련되어 있어, 서로가 서로를 컨트롤하는 상황에 빠져버리기 때문

- 이와 같이, 다수의 객체를 조정해야 하는 경우, Mediator 패턴을 사용한다
  - 각 표시 컨트롤의 로직을 중개인 안에만 기술한다

|이름|해설|
|----|----|
|Mediator|'중개인'의 인터페이스(API)를 정하는 인터페이스|
|Colleague|'멤버'의 인터페이스(API)를 정하는 인터페이스|
|ColleagueButton|Colleague 인터페이스를 구현. 버튼을 나타내는 클래스|
|ColleagueTextField|Colleague 인터페이스를 구현. 텍스트를 입력하는 클래스|
|ColleagueCheckbox|Colleague 인터페이스르 구현. 체크박스(여기서는 라디오버튼)를 나타내는 클래스|
|LoginFrame|Mediator 인터페이스를 구현. 로그인 다이얼로그를 나타내는 클래스|
|Main|동작 테스트용 클래스|  


### 클래스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch16-3.png){: width="100%" }


### 시퀀스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch16-4.png){: width="100%" }


### Mediator 인터페이스

- '중개인'을 표현하는 인터페이스
- createColleagues()
  - mediator가 관리하는 멤버를 생성하는 메소드
- colleagueChanged()
  - 각 멤버가 '상담'할 때 호출하는 메소드
  - 라디오 버튼이나 텍스트 필드의 상태가 변화했을 때, 이 메소드가 호출된다

```java
public interface Mediator {
  public abstract void createColleagues();
  public abstract void colleagueChanged();
}
```


### Colleague 인터페이스

- '중개인'에게 상담하러 오는 멤버를 나타내는 인터페이스
- 구체적인 멤버(ColleagueButton, ColleagueTextField, ColleagueCheckbox)는 이 인터페이스를 구현한다
- setMediator(Mediator mediator)
  - Mediator 인터페이스를 구현한 LoginFrame 클래스가 첫 번째로 호출하는 메소드
  - "내가 중개인이니까 기억해두세요"라는 의미
- setColleagueEnabled(boolean enabled)
  - "중개인이 내리는 지시"에 해당함
  - 멤버의 상태를 "유효" 또는 "뮤효"로 바꾸는 일을 수행한다

```java
public interface Colleague {
  public abstract void setMediator(Mediator mediator);
  public abstract void setColleagueEnabled(boolean enabled);
}
```

### ColleagueButton 클래스

- java.awt.Button의 하위 클래스
- setMediator(Mediator mediator)
  - 입력 인자로 들어온 Mediator 객체(Login Frame 클래스의 인스턴스)를 멤버 변수인 mediator에 할당함
- setColleagueEnabled(boolean enabled)
  - Button 클래스에서 물려받은 (Java의 GUI에서 정의되어 있는) setEnabled(boolean)를 호출하여 유효/무효를 설정한다
    - setEnabled(true)에서는 버튼은 누를 수 있지만, setEnabled(false)에서는 버튼을 누를 수 없음


```java
import java.awt.Button;

public class ColleagueButton extends Button implements Colleaue {
  private Mediator mediator;
  public ColleagueButton(String caption) {
    super(caption);
  }
  public void setMediator(Mediator mediator) {
    this.mediator = mediator;
  }
  public void setColleagueEnabled(boolean enabled) {
    setEnabled(enabled);
  }
}
```


### ColleagueTextField 클래스

- java.awt.TextField의 하위 클래스
- java.awt.event.TextListener 인터페이스도 구현함
  - 텍스트 필드의 내용이 바뀌었을 때, textValueChanged 메소드에서 이를 catch 하기 위한 리스너 역할도 한다
- setMediator(Mediator mediator)
  - 입력 인자로 들어온 Mediator를 멤버 변수인 mediator에 할당함
- setColleagueEnabled(boolean enabled)
  - TextField 클래스에서 물려받은 setEnabled(boolean)를 호출하여 유효/무효를 설정한다
  - 또한, setBackground 메소드를 이용하여 유효일 때는 백색, 무효일 때는 회색을 바꾼다
- textValueChanged(TextEvent e)
  - TextField의 내용이 바뀌었을 때, TextEvent가 발생되고 등록된 TextListener의 이 메소드가 자동으로 호출된다
  - 중개인의 colleagueChanged 메소드를 호출한다
  - 중개인에게 "내 문자열의 내용이 변경된 것을 보고합니다"라고 전함


```java
import java.awt.TextField;
import java.awt.Color;
import java.awt.event.TextListener;
import java.awt.event.TextEvent;

public class ColleagueTextField extends TextField implements TextListener, Colleague {
  private Mediator mediator;
  public Colleague TextField(String text, int columns) {
    super(text, columns);
  }
  public void seMediator(Mediator mediator) {
    this.mediator = meidator;
  }
  public void setColleagueEnabled(boolean enabled) {
    setEnabled(enabled);
    setBackground(enabled ? Color.white : Color.lightGray);
  }
  public void textValueChanged(TextEvent e) {
    mediator.colleagueChanged();
  }
}
```

### ColleagueCheckbox 클래스

- java.awt.Checkbox 클래스의 하위 클래스
- CheckboxGroup을 이용하여 라디오 버튼으로 사용한다
- java.awt.event.ItemListener 인터페이스도 구현함
  - 라디오 버튼의 상태변화를 itemStateChanged 메소드에서 캐치한다
- itemStateChanged(ItemEvent e)
  - 라디오버튼의 상태가 바뀌었을 때, ItemEvent가 발생되고, 등록된 ItemListener의 이 메소드가 자동으로 호출된다


```java
import java.awt.Checkbox;
import java.awt.CheckboxGroup;
import java.awt.event.ItemListener;
import java.awt.event.ItemEvent;

public class ColleagueCheckbox extends Checkbox implements ItemListener, Colleague {
  private Mediator mediator;
  public ColleagueCheckbox(String caption, CheckboxGroup group, boolean state) {
    super(caption, group, state);
  }
  public void setMediator(Mediator mediator) {
    this.mediator = mediator;
  }
  public void setColleagueEnabled(boolean enabled) {
    setEnabled(enabled);
  }
  public void itemStateChanged(ItemEvent e) {
    mediator.colleagueChanged();
  }
}
```


### LoginFrame 클래스

- java.awt.Frame 클래스의 하위 클래스
  - GUI 어플리케이션을 만들기 위한 클래스
- 중개인의 역할을 수행한다
- 생성자에서 하는 일
  - 배경색의 설정(setBackground() 이용)
    - 프레임을 구성하는 구성 요소의 배치를 결정한다
    - GridLayout(4,2): 프레임 영역을 4행 2열로 나눔
  - createColleagues 메소드에서 멤버들을 생성한다
  - colleague들을 배치한다 (add() 이용)
  - checkGuest의 초기 상태에 따라 프레임의 초기 상태를 설정한다
  - colleague 들을 보여준다
    - pack(): 윈도우를 컴포넌트를 고려하여 적합한 크기로 조절한다
    - show(): 윈도우를 보여준다
- createColleagues()
  - 각 colleague를 생성하고
  - Mediator를 설정한 후
  - 각 colleague에 해당 리스너를 연결한다
- colleagueChanged(Colleague c)
  - colleague의 상태가 변화했을 때, 호출되는 메소드
  - colleague의 상태 변화에 따라 해당 colleague의 상태를 어떻게 변화시킬 것인가에 대한 로직을 가지고 있다
  - 모든 colleague의 상담이 여기에 집중됨
- userpassChanged()
  - TextField 상태 변화에 따라 버튼 활성화/비활성화
- actionPerformed(ActionEvent e)
  - ActionListener가 되기위해서 구현하는 메소드
  - OK, Cancel 버튼이 눌러졌을 때 실행됨

```java
import java.awt.Frame;
import java.awt.Label;
import java.awt.Color;
import java.awt.CheckboxGroup;
import java.awt.GridLayout;
import java.awt.event.ActionListener;
import java.awt.event.ActionEvent;

public class LoginFrame extends Frame implements ActionListener, Mediator {
    private ColleagueCheckbox checkGuest;
    private ColleagueCheckbox checkLogin;
    private ColleagueTextField textUser;
    private ColleagueTextField textPass;
    private ColleagueButton buttonOk;
    private ColleagueButton buttonCancel;

    public LoginFrame(String title) {
        super(title);
        setBackground(Color.lightGray);
        setLayout(new GridLayout(4, 2));
        createColleagues();
        add(checkGuest);
        add(checkLogin);
        add(new Label("Username:"));
        add(textUser);
        add(new Label("Password:"));
        add(textPass);
        add(buttonOk);
        add(buttonCancel);
        colleagueChanged();
        pack();
        show();
    }

    public void createColleagues() {
        CheckboxGroup g = new CheckboxGroup();
        checkGuest = new ColleagueCheckbox("Guest", g, true);
        checkLogin = new ColleagueCheckbox("Login", g, false);
        textUser = new ColleagueTextField("", 10);
        textPass = new ColleagueTextField("", 10);
        textPass.setEchoChar('*');
        buttonOk = new ColleagueButton("OK");
        buttonCancel = new ColleagueButton("Cancel");

        checkGuest.setMediator(this);
        checkLogin.setMediator(this);
        textUser. setMediator(this);
        textPass. setMediator(this);
        buttonOk. setMediator(this);
        buttonCancel.setMediator(this);

        checkGuest.addItemListener(checkGuest);
        checkLogin.addItemListener(checkLogin);
        textUser.addTextListener(textUser);
        textPass.addTextListener(textPass);
        buttonOk.addActionListener(this);
        buttonCancel.addActionListener(this);
    }


    public void colleagueChanged() {
        if (checkGuest.getState()) {
            textUser.setColleagueEnabled(false);
            textPass.setColleagueEnabled(false);
            buttonOk.setColleagueEnabled(true);
        } else {
            textUser.setColleagueEnabled(true);
            userpassChanged();
        }
    }

    private void userpassChanged() {
        if (textUser.getText().length() > 0) {
            textPass.setColleagueEnabled(true);
            if (textPass.getText().length() > 0) {
                buttonOk.setColleagueEnabled(true);
            } else {
                buttonOk.setColleagueEnabled(false);
            }
        } else {
            textPass.setColleagueEnabled(false);
            buttonOk.setColleagueEnabled(false);
        }
    }
    public void actionPerformed(ActionEvent e) {
        System.out.println(e.toString());
        System.exit(0);
    }
}
```

### Main 클래스

- LoginFrame의 인스턴스를 생성함

```java
import java.awt.*;
import java.awt.event.*;

public class Main {
  public static vodi main(String args[]) {
    new LoginFrame("Mediator Sample");
  }
}
```


## 03. 등장 역할

- 클래스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch16-5.png){: width="100%" }

- Mediator(조정자, 중개자)의 역할
  - Colleague 역할들과 통신을 조정하기 위한 인터페이스를 정하는 역할
  - 예제에서는 Mediator 인터페이스가 해당됨

- ConcreteMediator(구체적인 조정자, 중개자)의 역할
  - Mediator 역할의 인터페이스(API)를 구현하여, 실제의 조정을 수행하는 역할
  - 예제에서는 LoginFrame 클래스가 해당됨

- Colleague(동료)의 역할
  - Mediator 역할과 통신을 할 인터페이스(API)를 정함
  - 예제에서는 Colleague 인터페이스가 해당됨

- ConcreteColleague(구체적인 동료)의 역할
  - Colleague 역할의 인터페이스(API)를 구현함
  - 예제에서는, ColleagueButton, ColleagueTextField, ColleagueCheckbox가 해당됨


## 04. 독자의 사고를 넓혀주는 힌트

- 분산이 재앙이 되는 경우
  - ColleagueButton, ColleagueTextField, ColleagueCheckbox 들 사이에 상태 변화 로직이 Mediator인 LoginFrame에 집중되어 있다
  - 이 로직이, 각각의 Colleague에 분산되어 있다면 처리하는 일, 즉 "divide and conquer"가 많이 사용된다
  - 이 경우는, 반대로 한군데 로직을 집중시킨다
  - 분산시킬 것은 분산시키고, 집중시킬 것은 집중시키는 것이 중요하다

- 재사용할 수 있는 것은 무엇인가?
  - ConcreteColleague 역할을 하는 ColleagueButton, ColleagueTextField, ColleagueCheckbox는 다른 대화창에서도 재사용 가능하다
    - 특정 어플리케이션 관련 로직을 이 클래스들이 포함하고 있지 않기 때문
  - ConcreteMediator 역할을 하는 LoginFrame 클래스는 재사용하기 힘들다
    - 특정 어플리케이션 관련 로직을 포함하고 있기 때문


## 05. 관련 패턴

- Facade 패턴
- Observer 패턴


## 06. 요약

- 믿을 수 있는 중개인이, 복잡하게 얽혀 있는 객체들 간의 상호통신을 중지시키고, 객체들 간의 상호 작용 로직을 MEdiator에 집중시킴으로써 처리르 원활하게 한다
- 특히, GUI 어플리케이션에서 효과적이다


## 연습 문제

- 16-1
  - 사용자 로그인일 때, 사용자 명과 패스워드 둘 다 네문자 이상인 경우에만 OK 버튼이 유요하도록 예제 프로그램을 수정하기

- 16-2
  - ColleagueButton, ColleagueTextField, ColleagueCheckbox가 공통으로 가지고 있는 setMediator()와 mediator 필드를 Colleague 인터페이스 안에 넣어서 구현할 수 있는가?


## Homework3: Mediator 패턴 응용

- 16장 예제 프로그램에, 다음 Colleague와 로직을 추가할 것
  - 추가할 Colleague
    - 라디오 박스
      - "member": "login" 라디오 박스 옆에 추가
    - 텍스트 필드
      - "주민등록번호": "Password" 필드 아래에 추가
  - 추가할 로직
    - 사용자가 "member" 라디오 박스를 선택 시,
      - Username, Password, 주민등록번호 텍스트필드가 활성화된다
      - 그 이외에는 주민등록번호 텍스트필드는 비활성화된다
      - OK 버튼은 비활성화되어 있다가, 사용자가 주민등록번호 13자리를 모두 입력하면 OK 버튼이 활성화된다
        - 주민등록번호는 모두 숫자로 이루어져야 한다
        - 13자리 중에서 숫자 이외의 문자가 포함되어 있으면, OK 버튼을 계속해서 비활성화 상태로 유지한다


















