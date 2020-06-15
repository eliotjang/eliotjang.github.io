---
title: "[시스템분석및설계] Chapter 19. State 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch19. State 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-06-14T14:00:00+09:00  
---  

## 01. State 패턴  

- 어떤 것을 클래스로 표현할지는 설계하는 사람의 마음이다 
- 클래스에 대응하는 구체적인 '사물'이 현실에 존재하는 경우도 있고, 존재하지 않는 경우도 있다 
- State 패턴은, '상태'를 클래스로 표현한 것이다 
  - 클래스를 교체함으로써, '상태의 변화'를 나타낼 수 있고,
  - 새로운 상태를 추가해야 할 때 무엇을 프로그램하면 되는지 명확해 진다  

## 02. 예제 프로그램  

- 금고경비 시스템 
  - 시간마다 경비의 상태가 변하는 금고경비 시스템 
  - 호출 상황을 화면에 표시한다 
  - 프로그램상의 1초를 현실 세계의 1시간으로 가정한다  

![](https://eliotjang.github.io/assets/images/system-analysis/ch19-1.png){: width="100%"}

`금고가 하나 있습니다`  
`금고는 경비센터와 접속되어 있습니다`  
`금고에는 비상벨과 일반 통화용의 전화가 접속되어 있습니다`  
`금고에는 시계가 붙어 있어 현재의 시간을 감시하고 있습니다`  
`주간은 9:00 ~ 16:59, 야간은 17:00 ~ 23:59 및 0:00 ~ 8:58입니다`  
`금고는 주간에만 사용할 수 있습니다`  
`주간에 금고를 사용하면 경비센터에 사용 기록이 남습니다`  
`야간에 금고를 사용하면 경비센터에 비상사태의 통보가 갑니다`  
`비상벨은 언제라도 사용할 수 있습니다`  
`비상벨을 사용하면 경비센터에 비상벨의 통보가 갑니다`  
`일반 통화용의 전화는 언제라도 사용할 수 있습니다(그러나 야간은 녹음만)`  
`주간에 전화를 사용하면 경비센터가 호출됩니다`  
`야간에 전화를 사용하면 경비센터의 자동응답기가 호출됩니다`  


- State 패턴을 사용하지 않는 의사 코드(pseudo code)  

![](https://eliotjang.github.io/assets/images/system-analysis/ch19-2.png){: width="70%"}  

- State 패턴을 사용한 의사 코드(pseudo  code)
  - 앞의 코드와 달리, 파묻혀 있던 '상태'를 외부로 끌어냄 
    - 따라서, <span style="color:red">상태를 체크하기 위한 if 문이 없다</span>  


![](https://eliotjang.github.io/assets/images/system-analysis/ch19-3.png){: width="100%"}  


|이름|해설|
|----|----|
|State|금고의 상태를 나타내는 인터페이스|
|DayState|State를 구현하고 있는 클래스. 주간의 상태를 나타냄|
|NightState|State를 구현하고 있는 클래스. 야간의 상태를 나타냄|
|Context|금고의 상태변화를 관리하고, 경비센터와의 연락을 취하는 인터페이스|
|SafeFrame|Context를 구현하고 있는 클래스. 버튼이나 화면표시 등의 사용자 인터페이스를 가짐|
|Main|동작 테스트용 클래스|  


![](https://eliotjang.github.io/assets/images/system-analysis/ch19-4.png){: width="100%"}  


### State 인터페이스  

- 금고의 상태를 나타냄 
- 다음 이벤트가 호출되는 인터페이스(API)를 규정함 
  - 시간이 설정되었을 때 ⇒ doClock()을 호출함 
  - 금고가 사용되었을 때 ⇒ doUse()를 호출함 
  - 비상벨이 울렸을 때 ⇒ doAlarm()을 호출함 
  - 일반 통화를 할 때 ⇒ doPhone()을 호출함 
- 각 메소드의 형식 매개변수 Context 
  - 상태를 관리하거나 실제 경비센터를 호출하는 일을 하는 클래스  

```java
public interface State {
  public abstract void doClock(Context context, int hour);
  public abstract void doUse(Context context);
  public abstract void doAlarm(Context context);
  public abstract void doPhone(Context context);
}
```  


### DayState 클래스  

- 주간의 상태를 나타내는 클래스 
- 하나의 상태만 필요하므로, Singleton 패턴을 사용함 
  - DayState 타입의 객체를 static으로 선언하고 인스턴스 한 개를 생성함 
  - 생성자를 private으로 선언함 
- doClock() : 시간을 설정하는 메소드 
  - 매개변수로 제공된 시간이 야간의 시간이면, 시스템의 상태를 야간으로 바꾼다 
- "주간 상태"에서 하는 일을 표현하는 메소드 (Context의 메소드를 이용한다) 
  - doUse() : 주간에 금고를 사용했음을 기록 
  - doAlarm() : 경비센터를 호출함 
  - doPhone() : 경비센터에 일반통화를 함  

```java
public class DayState implements State { 
  private static DayState singleton = new DayState();
  private DayState() {
  }
  public static State getInstance() {
    return singleton;
  }
  public void doClock(Context context, int hour) {
    if (hour < 9 || 17 <= hour) {
      context.changeState(NightState.getInstance());
    }
  }
  public void doUse(Context context) {
    context.recordLog("금고사용(주간)");
  }
  public void doAlarm(Context context) {
    context.callSecurityCenter("비상벨(주간)");
  }
  public void doPhone(Context context) {
    context.callSecurityCenter("일반통화(주간)");
  }
  public String toString() {
    return "[주간]";
  }
}
```  


### NightState 클래스  

- 야간의 상태를 나타내는 클래스 
- DayState 클래스와 비슷하다  


```java
public class NightState implements State {
  private static NightState singleton = new NightState();
  private NightState() {
  }
  public static State getInstance() {
    return singleton;
  }
  public void doClock(Context context, int hour) {
    if (9 <= hour && hour < 17) {
      context.changeState(DayState.getInstance());
    }
  }
  public void doUse(Context context) {
    context.callSecurityCenter("비상 : 야간금고 사용!");
  }
  public void doAlarm(Context context) {
    context.callSecurityCenter("비상벨(야간)");
  }
  public void doPhone(Context context) {
    context.recordLog("야간통화 녹음");
  }
  public String toString() {
    return "[야간]";
  }
}
```  


### Context 인터페이스  

- 상태를 관리하거나 경비센터를 실제로 호출하는 클래스를 위한 인터페이스를 제공한다  


```java
public interface Context {
  public abstract void setClock(int hour);
  public abstract void changeState(State state);
  public abstract void callSecurityCenter(String msg);
  public abstract void recordLog(String msg);
}
```  


### SafeFrame 클래스  

- GUI를 사용해서, 금고경비 시스템을 구현한다 
- Context 인터페이스를 구현함 
- state 필드 : 금고의 현재 상태를 저장하는 변수 
  - 초기는 '주간'상태임 
- 생성자에서 하는 일 
  - 배경색의 설정 
  - 레이아웃 매니저의 설정 
  - 패널의 배치 
  - 리스너(Listener)의 설정 
    - addActionListener 메소드를 이용하여 ActionListener를 구현한 객체를 버튼에 등록한다 
    - 버튼이 눌러지면 등록된 ActionListener 객체의 actionPerformed()를 호출한다  
- 버튼의 ActionListener는 SafeFrame 자신이다 
- actionPerformed()
  - 눌러진 버튼의 종류에 따라 state.doUse(this) / state.doAlarm(this) / state.doPhone(this) 중 하나를 호출한다 
  - 현재 상태가 주간인지 야간인지 체크하는 코드가 필요없다 
- setClock()
  - 시간 설정을 위해서 클라이언트(Main)가 호출하는 메소드 
- callSecurityCenter()
  - 경비센터에 대한 호출을 표현함 
- recordLog() 
  - 경비센터의 로그에 기록하는 일을 표현함  

```java
import java.awt.Frame;
import java.awt.Label;
import java.awt.Color;
import java.awt.Button;
import java.awt.TextField;
import java.awt.TextArea;
import java.awt.Panel;
import java.awt.BorderLayout;
import java.awt.event.ActionListener;
import java.awt.event.ActionEvent;

public class SafeFrame extends Frame implements ActionListener, Context {
    private TextField textClock = new TextField(60);
    private TextArea textScreen = new TextArea(10, 60);
    private Button buttonUse = new Button("금고사용");
    private Button buttonAlarm = new Button("비상벨");
    private Button buttonPhone = new Button("일반통화");
    private Button buttonExit = new Button("종료");

    private State state = DayState.getInstance();


    public SafeFrame(String title) {
        super(title);
        setBackground(Color.lightGray);
        setLayout(new BorderLayout());

        add(textClock, BorderLayout.NORTH);
        textClock.setEditable(false);

        add(textScreen, BorderLayout.CENTER);
        textScreen.setEditable(false);

        Panel panel = new Panel();
        panel.add(buttonUse);
        panel.add(buttonAlarm);
        panel.add(buttonPhone);
        panel.add(buttonExit);

        add(panel, BorderLayout.SOUTH);

        pack();
        show();

        buttonUse.addActionListener(this);
        buttonAlarm.addActionListener(this);
        buttonPhone.addActionListener(this);
        buttonExit.addActionListener(this);
    }

    public void actionPerformed(ActionEvent e) {
        System.out.println(e.toString());
        if (e.getSource() == buttonUse) {
            state.doUse(this);
        } else if (e.getSource() == buttonAlarm) {
            state.doAlarm(this);
        } else if (e.getSource() == buttonPhone) {
            state.doPhone(this);
        } else if (e.getSource() == buttonExit) {
            System.exit(0);
        } else {
            System.out.println("?");
        }
    }

    public void setClock(int hour) {
        String clockstring = "현재 시간은";
        if (hour < 10) {
            clockstring += "0" + hour + ":00";
        } else {
            clockstring += hour + ":00";
        }
        System.out.println(clockstring);
        textClock.setText(clockstring);
        state.doClock(this, hour);
    }

    public void changeState(State state) {
        System.out.println(this.state + "에서" + state + "로 상태가 변화했습니다.");
        this.state = state;
    }

    public void callSecurityCenter(String msg) {
        textScreen.append("call! " + msg + "\n");
    }

    public void recordLog(String msg) {
        textScreen.append("record ... " + msg + "\n");
    }
}
```


### Main 클래스  

- SafeFrame 인스턴스를 한 개 만든 후,
- 1초 간격으로 SafeFrame의 setClock() 메소드를 호출한다 
  - Thread.sleep(1000) 문장을 사용함 : (1000/1000)1초 동안 CPU를 반환하고 쉬겠다는 뜻  


```java
public class Main {
  public static void main(String[] args) {
    SafeFrame frame = new SafeFrame("State Sample");
    while (true) {
      for (int hour = 0; hour < 24; hour++) {
        frame.setClock(hour);
        try {
          Thread.sleep(1000);
        } catch (InterruptedException e) {
        }
      }
    }
  }
}
```


![](https://eliotjang.github.io/assets/images/system-analysis/ch19-5.png){: width="100%"}  

- "금고사용" 버튼이 눌려진 후에 doUse()를 실행하는 모습 

![](https://eliotjang.github.io/assets/images/system-analysis/ch19-6.png){: width="100%"}  


## 03. 등장 역할  

![](https://eliotjang.github.io/assets/images/system-analysis/ch19-7.png){: width="100%"}  


- State(상태)의 역할 
  - 상태를 나타내는 역할 
  - 각 상태에 따라 다른 행동을 하는 통일된 인터페이스(API)를 결정함 
  - 예제에서는, State 인터페이스가 해당됨 

- ConcreteState(구체적인 상태)의 역할 
  - 구체적인 개개의 상태를 표현하는 역할 
  - State 역할이 결정한 인터페이스를 구현함 
  - 예제에서는, DayState와 NightState 클래스가 해당됨  

- Context(상황, 전후관계, 문맥)의 역할 
  - 현재의 상태를 나타내는 ConcreteState 역할을 가지고 있음 
  - State 패턴 이용자가 필요로 하는 인터페이스를 결정함 
  - 예제에서는 Context 인터페이스와 SafeFrame 클래스가 해당됨  



## 04. 독자의 사고를 넓혀주는 힌트  

- 분할해서 통치하라 
  - divide and conquer 
     - 복잡하고 규모가 큰 프로그램을 다룰 때, 우선 작은 문제로 나누어라. 그래도 풀기 어려우면 더 작은 문제로 나누어라 
  - State 패턴에서는 개개의 구체적인 상태를 각각 클래스로 나누어서 표현함으로써 문제를 분할함 
  - 상태의 종류가 많을수록 유용함 
    - State 패턴을 사용하지 않는다면, 계속해서 상태를 검사하는 조건문이 필요하다 
    - 예 : 상태가 10가지라면, if-else-if 문이 10개 정도 필요함 

- 상태에 의존한 처리 
  - Main 클래스가 SafeFrame의 setClock() 메소드를 호출해서 시간을 설정해달라고 부탁함 
  - setClock() 메소드 안에서는, state.doClock(this, hour)를 호출하여 현재 state에 그 처리를 위임함 
  - 현재 state가 무엇이냐에 따라 행동이 달라진다. 즉, "상태에 따라 행동이 달라지는 처리"이다  

- 새로운 상태를 추가하는 것은 간단 
  - 예제 프로그램에서는, State 인터페이스를 구현한 XXXState 클래스를 만들어 필요한 메소드를 구현하기만 하면 된다  


## 05. 관련 패턴  

- Singleton 패턴 
- Flyweight 패턴  


## 06. 요약  

- 시스템의 각 상태를 클래스로 표현한 State 패턴  


## 연습 문제  

- 19-1
  - Context 인터페이스를 추상 클래스로 정의하지 않은 이유는? 
- 19-2
  - 주간, 야간 범위를 변경하는 경우, 코드 수정은 어떻게 해야 하는가?
- 19-3
  - "점심시간"이라는 상태를 추가하시오 
- 19-4
  - "비상시"라는 상태를 추가하시오  


## Homwork#5: State 패턴 응용  

- 연습문제 19-3의 답에, 다음과 같은 새로운 상태 추가하기 
  - 상태 : "야식시간" (23:00 ~ 24:00)
    - 금고를 사용하면, 경비센터에 기록이 남고, 비상사태 통보가 간다 
    - 비상벨을 사용하면, 경비센터에 비상벨 통보가 간다 
    - 전화를 사용하면, 경비센터의 자동응답기가 호출된다 
- 숙제 제출 방법 : 이전 것과 똑같은 방법으로 제출
- 숙제 검사 방법 : 
  - GUI에서, 각 버튼을 '야식시간'일 때 누르면, 위와 같은 요구 사항에 해당하는 메시지가 textScreen에 출력되어야 한다  
















