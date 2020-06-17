---
title: "[시스템분석및설계] Chapter 22. Command 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch22. Command 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-06-17T22:00:00+09:00  
---  

## 01. Command 패턴  

- 클래스가 일을 처리할 때는,
  - 자신의 클래스나 다른 클래스의 메소드를 호출한다 
- Command 패턴에서는, 실행하고자 하는 일이 
  - 메소드 호출이 아닌, *'명령을 나타내는 클래스'*의 인스턴스 생성으로 표현된다 
- 이점 
  - history를 관리하고 싶을 때, Command 클래스의 인스턴스의 집합을 관리하면 된다 
  - 명령의 집합을 보존해 두면, 똑같은 명령을 재실행 할 수도 있고, 또는 여러 개의 명령을 모든 것을 새로운 명령으로서 재사용할 수도 있다  


## 02. 예제 프로그램  

- 간단한 그림 그리기 프로그램 
  - 마우스를 drag 하면 빨간 점이 연결되어 그림이 그려진다 
  - 'clear' 버튼을 누르면 점이 모두 지워진다 
  - 사용자가 마우스를 드래그할 때 마다, '이 위치에 점을 그려라'라는 명령이 DrawCommand 클래스의 인스턴스로 생성된다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch22-1.png){: width="80%"}  
  - 
|패키지|이름|해설|
|------|----|----|
|command|Command|'명령'을 표현하는 인터페이스|
|command|MacroCommand|'여러 개의 명령을 모은 명령'을 나타내는 클래스|
|drawer|DrawCommand|'점 그리기 명령'을 표현한 클래스|
|drawer|Drawale|'그리기 대상'을 표현한 인터페이스|
|drawer|DrawCanvas|'그리기 대상'을 구현한 클래스|
|Anonymous|Main|동작 테스트용 클래스|  


![](https://eliotjang.github.io/assets/images/system-analysis/ch22-2.png){: width="100%"}  


### Command 인터페이스  

- '명령'을 표현하기 위한 인터페이스 
- execute()
  - 무언가를 실행하는 메소드 
  - 구체적으로 무슨 일을 하는지는 Command 인터페이스를 구현한 클래스가 결정한다 
- MacroCommand와 DrawCommand가 이 인터페이스를 구현한다  


```java
package command;

public interface Command {
  public abstract void execute();
}
```  


### MacroCommand 클래스  

- '여러 개의 명령을 한데 모은 명령'을 나타냄 
- Composite 패턴이 사용됨 
  - 여러 개의 명령을 모은 것(container)이면서, 그 자체가 하나의 명령(content)가 된다 
- command 필드 
  - java.util.Stack 형 
  - 복수의 Command를 모아둠 
- execute()
  - 자신이 가지고 있는 모든 명령의 execute()을 호출한다(실행한다)
  - 자신이 가지고 있는 명령이 MacroCommand이면, 그 MacroCommand가 가지고 있는 명령들의 execute()이 차례대로 실행된다(recursive call)
- append()
  - MacroCommand 클래스에 새로운 Command(Command 인터페이스를 구현한 클래스의 인스턴스)를 추가하는 메소드 
  - Stack 클래스의 push()를 이용함
  - 실수로 자기 자신을 추가하지 않도록 체크함
    - 자기 자신이 추가되면, execute() 실행 시 무한 루프가 돌게 된다 
- undo() 메소드 
  - commands의 마지막 명령을 삭제하는 메소드 
  - Stack 클래스의 pop()를 이용함 
- clear() 메소드 
  - commands의 모든 명령을 삭제하는 메소드  


```java
package command;

import java.util.Stack;
import jaav.util.Iterator;

public class MacroCommand implements Command {
  private Stack commands = new Stack();
  public void execute() {
    Iterator it = commands.iterator();
    while (it.hasNext()) {
      ((Command)it.next()).execute();
    }
  }
  public void append(Command cmd) {
    if (cmd != this) {
      commands.push(cmd);
    }
  }
  public void undo() {
    if (!commands.empty()) {
      commands.pop();
    }
  }
  public void clear() {
    commands.clear();
  }
}
```  


### DrawCommand 클래스  

- Command 인터페이스 구현 
- '점 그리기 명령'을 표현함 
- drawable 필드 : 그림 그리기를 실행할 대상(객체)을 저장함 
- position 필드 : 그림 그리기를 행할 위치를 나타냄 
  - java.awt.Point 클래스 : X좌표와 Y좌표를 갖는 클래스 
    - 2차원 평면 상의 위치를 나타냄 
- 생성자의 매개변수 
  - drawable : 어떤 개체를 대상으로하는 그리기 명령인가를 나타내기 위한 인자 
  - position : 어디에(어느 좌표에) 그리는 명령인가를 나타내기 위한 인자 
  - '이 위치에 점을 그려라'라는 명령을 생성하는 생성자 
- execute() 메소드 
  - drawable 필드의 draw 메소드를 호출함 
  - 실제 그리기를 실행하는 메소드  


```java
package drawer;

import command.Command;
import java.awt.Point;

public class DrawCommand implements Command {
  protected Drawable drawable;
  private Point position;
  public DrawCommand(Drawable drawable, Point position) {
    this.drawable = drawable;
    this.position = position;
  }
  public void execute() {
    drawable.draw(position.x, position.y);
  }
}
```  


### Drawable 인터페이스  

- '그림 그리기 대상'을 표현함 
- draw(int x, int y) 메소드를 가진다 
  - x, y : 그림 그릴 때의 좌표를 나타냄  


```java
package drawer;

public interface Drawable {
  public abstract void draw(int x, int y);
}
```  


### DrawCanvas 클래스  

- Drawable 인터페이스를 구현하고, java.awt.Canvas를 상속함 
- history 필드 
  - 지금까지 실행한 그림 그리기 명령어들의 집합을 가지고 있음 
- 생성자 
  - 폭, 높이, 그림 그리기 이력(history)을 받아서, DrawCanvas 인스턴스를 초기화한다 
- paint()
  - DrawCanvas를 다시 그릴 필요가 생겼을 때 java.awt 프레임워크로부터 자동으로 호출되는 메소드 
  - repaint() 메소드가 호출되면, 자동으로 paint() 메소드가 실행된다 
  - history가 보관하고 있는 모든 그리기 명령들을 실행한다 
- draw()
  - Graphics 객체를 얻어서,
  - 색깔을 빨간색으로 지정하고
    - 397페이지 : `private Color color = Color.red;`
  - Graphics 객체의 fillOval(x, y, 사각형 가로, 사각형 세로)을 이용하여 원을 그린다  


```java
package drawer;

import command.*;

import java.util.*;
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;

public class DrawCanvas extends Canvas implements Drawable {
  private Color color = Color.red;
  private int radius = 6;
  private MacroCommand history;
  public DrawCanvas(int width, int height, MacroCommand history) {
    setSize(width, height);
    setBackground(Color.white);
    this.history = history;
  }
  public void paint(Graphics g) {
    history.execute();
  }
  public void draw(int x, int y) {
    Graphics g = getGraphics();
    g.setColor(color);
    g.fillOval(x - radius, y - radius, radius * 2, radius * 2);
  }
}
```  


### Main 클래스  

- 예제 프로그램을 동작시키는 클래스 
- JFrame을 상속받고, ActionListener, MouseMotionListener, WindowListener 인터페이스를 구현함 
- history 필드 
  - DrawCanvas 생성 시에 인자로 넘겨줌 
    - ⇒ Main 인스턴스와 DrawCanvas 인스턴스가 history를 공유한다 
- canvas 필드 
  - 그림 그리는 영역을 나타냄 
- clearButton 필드 
  - javax.swing.JButton 클래스 
  - 그린 점들을 모두 지우는 버튼 
- 생성자 
  - 버튼 클릭, 마우스 클릭 등의 이벤트를 받아들이는 리스너를 설정함 
  - 여러 가지 GUI 부품을 배치함 
    - Box 객체를 이용함 
    - Box : a lightweight container that uses a BoxLayout object as its layout manager
      - BoxLayout.X_AXIS : 가로 배치 시 사용 
      - BoxLayout.Y_AXIS : 세로 배치 시 사용  
  - 
![](https://eliotjang.github.io/assets/images/system-analysis/ch22-3.png){: width="80%"}  
- actionPerformed()
  - clearButton이 눌러졌을 때 호출되는 메소드 
  - history에 보관되어 있던 모든 명령을 지우고, 캔버스를 다시 그린다 
    - 그 결과, 캔버스의 내용이 사라진다 
- mouseDragged()
  - 사용자가 마우스를 drag하면 이 메소드가 호출된다 
    - 그리기 명령을 나타내는 DrawCommand 객체를 생성한 후,
    - 이를 history에 추가하고,
    - cmd.execute()를 호출하여 지정 위치에 빨간 점을 그린다 
- windowClosing()
  - 창의 오른쪽 아이콘 중에 ☒를 눌렀을 때 호출되는 메소드 
  - System.exit()을 이용하여 프로그램을 종료한다  


```java
import command.*;
import drawer.*;

import java.awt.*;
import java.awt.event.*;
import javax.swing.*;

public class Main extends JFrame implements ActionListener, MouseMotionListener, WindowListener {
  private MacroCommand history = new MacroCommand();
  private DrawCanvas canvas = new DrawCanvas(400, 400, history);
  private JButton clearButton = new JButton("clear");
  public Main(String title) {
    super(title);

    this.addWindowListener(this);
    canvas.addMouseMotionListener(this);
    clearButton.addActionListener(this);

    Box buttonBox = new Box(BoxLayout.X_AXIS);
    buttonBox.add(clearButton);
    Box mainBox = new Box(BoxLayout.Y_AXIS);
    mainBox.add(buttonBox);
    mainBox.add(canvas);
    getContentPane().add(mainBox);

    pack();
    show();
  }
  public void actionPerformed(ActionEvent e) {
    if (e.getSource() == clearButton) {
      history.clear();
      canvas.repaint();
    }
  }
  public void mouseMoved(MouseEvent e) {
  }
  public void mouseDragged(MouseEvent e) {
    Command cmd = new DrawCommand(canvas, e.getPoint());
    history.append(cmd);
    cmd.execute();
  }
  public void windowClosing(WindowEvent e) {
    System.exit(0);
  }
  public void windowActivated(WindowEvent e) {}
  public void windowClosed(WindowEvent e) {}
  public void windowDeactivated(WindowEvent e) {}
  public void windowDeiconified(WindowEvent e) {}
  public void windowIconified(WindowEvent e) {}
  public void windowOpened(WindowEvent e) {}
  
  public static void main(String[] args) {
    new Main("Command Pattern Sample");
  }
}
```  


- 예제 프로그램 행동 방식 
  - 마우스가 드래그 될 때마다, 어느 좌표에 빨간 점이 그려졌다는 명령을 나타내는 DrawCommand 명령어가 history에 추가된다 
    - 이력을 유지함 
    - redo나 undo에 사용할 수 있다(연습문제 22-2 및 숙제#6 참조)
  - clearButton이 눌려지면, history가 사라진다  


### 시퀀스 다이어그램  

![](https://eliotjang.github.io/assets/images/system-analysis/ch22-4.png){: width="100%"}  


## 03. 등장 역할  

- Command(명령)의 역할 
  - 명령의 인터페이스(API)를 정의하는 역할 
  - 예제에서는 Command 인터페이스가 해당됨 
- ConcreteCommand(구체적인 명령)의 역할 
  - Command 인터페이스를 구현하고 있는 역할 
  - 예제에서는, MacroCommand 클래스와 DrawCommand 클래스가 해당됨 
- Receiver(수신자)의 역할 
  - Command 명령을 실행할 때 대상이 되는 역할 
  - 명령을 받아들이는 사람 또는 객체 
  - 예제에서는, DrawCanvas가 해당됨 
- Client(의뢰자)의 역할 
  - ConcreteCommand를 생성하고, Receiver를 할당하는 역할 
  - 예제에서는, Main이 해당됨 
- Invoker(기동자)의 역할 
  - 명령을 처음 실행하는 역할 
  - 예제에서는, Main과 DrawCanvas 클래스가 해당됨 
    - history의 execute()을 호출함  


![](https://eliotjang.github.io/assets/images/system-analysis/ch22-5.png){: width="80%"}  


![](https://eliotjang.github.io/assets/images/system-analysis/ch22-6.png){: width="80%"}  


## 04. 독자의 사고를 넓혀주는 힌트  

- 명령이 지니고 있어야 하는 정보는? 
  - 예제에서는, DrawCommand는 그리기 대상과 점의 위치에 대한 정보만 가진다 
  - 필요에 따라서, 점의 크기나 색, 모양, 이벤트 발생 시각 등에 대한 정보를 지니게 할 수도 있다 
- 이력의 보존 
  - history 멤버 변수는 MacroCommand 타입으로, 지금까지 발생된 모든 그리기 명령, 즉 지금까지의 모든 그리기 정보를 가지고 있다 
  - 이 멤버 변수를 파일로 보존하면, '그리기 이력'이 보존된다 
- 어댑터 
  - Main 클래스가 구현하는 인터페이스 중 WindowListener 인터페이스는 다음과 같은 메소드를 선언하고 있다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch22-7.png){: width="60%"}  
  - 이 인터페이스를 구현하는 클래스는, 위의 7가지 메소드를 모두 구현해야 한다 
    - 즉, windowClosing()만 구현하고자 할 때에도, 나머지 6개 메소드에 대한 구현도 제공해 주어야 한다 
      - 예제 프로그램에서도 나머지 6개 메소드는 빈 공간이긴 하지만 구현해주고 있다 
  - 이러한 불편한 문제를 해결하기 위해서, 어댑터라고 불리는 클래스들이 java.awt.event 패키지에 제공된다 
  - 
|인터페이스|어댑터|
|----------|------|
|MouseMotionListener 인터페이스|MouseMotionAdapter 클래스|
|WindowListener 인터페이스|WindowAdapter 클래스|  
    - WindowAdapter 안에서의 각 메소드는 아무것도 하지 않는 빈칸으로 구현되어 있다 
    - WindowListener 역할을 하기 위해서는, WindowAdapter 클래스를 상속받고, 원하는 메소드만 oevrride(재정의)하면 된다 
  - 익명의 inner class(내부 클래스)를 이용하면 어댑터를 좀 더 깔끔하게 사용할 수 있다 
    - 리스트 22-7 MouseMotionListener 인터페이스를 사용한 경우 
      - 빈 칸인 mouseMoved()도 구현하였다 
    - 리스트 22-8 MouseMotionAdapter 클래스를 사용한 경우 
      - MouseMotionAdapter의 하위 클래스(이름은 없음)의 인스턴스를 생성함 
        - 필요없는 mouseMoved()의 구현은 제공하지 않고,
        - mouseDragged() 메소드만 재정의하는 익명의 내부 클래스의 객체를 생성하였다 
    - 
![](https://eliotjang.github.io/assets/images/system-analysis/ch22-8.png){: width="80%"}  
  - 익명의 inner 클래스를 컴파일한 경우에 다음과 같은 파일명의 파일이 생성된다 
    - `Main$1.class`
    - 
![](https://eliotjang.github.io/assets/images/system-analysis/ch22-8.png){: width="80%"}  



## 05. 관련 패턴 

- Composite 패턴 
- Memento 패턴 
- Prototype 패턴  


## 06. 요약  

- '명령'을 객체로 표현해서 이력을 보관하기도 하고, 재실행을 할 수도 있는 Command 패턴  


## 연습 문제  

- 22-1
  - '그림 그리기 색을 설정한다'라는 기능을 추가함
    - ColorCommand 클래스 작성 
- 22-2
  - '마지막으로 그린 점을 삭제한다'라는 기능을 추가함 
    - undo 버튼 추가 
    - undo 버튼이 눌리면 history.undo 호출하여 repaint
- 22-3
  - MouseMotionListener, WindowListener 인터페이스 대신 MouseMotionAdapter, WindowAdapter 클래스 사용  


## Homework #6: Command 패턴 응용  

- 연습문제 22-1 + 22-2에 redo 기능 추가하기 
  - 색깔 지정도 되면서 undo/redo 기능이 되는 프로그램 
  - 'redo' 버튼을 undo 버튼 오른쪽에 추가한다 
  - 프로그램 동작 방식 
    - 색깔을 바꾸면서 그림을 그리다가(점을 찍다가)
    - undo 버튼을 누를 때마다, 최근 점부터 사라진다 
    - redo 버튼을 누를 때마다, 마지막의 undo 버튼이 눌러졌을 대 지워졌던 점부터 *지워졌을 당시의 위치와 색깔을 그대로 하여* 다시 그려진다 
  - 힌트 : 
    - undo를 행했던 Command들을 유지하는 멤버 변수 undoHistoryForRedo와 적당한 메소드들을 DrawCanvas 클래스에 추가한다 
    - Main.main()의 actionPerformed()에서는
      - 눌러진 버튼이 'redo' 버튼인 경우 적당한 일을 실행한다 
- 숙제 제출 방법 : 이전 것과 똑같은 방법으로 제출  



















