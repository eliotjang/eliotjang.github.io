---
title: "[시스템분석및설계] Chapter 18. Memento 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch18. Memento 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
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




































