---
title: "[시스템분석및설계] Chapter 10. 전략(Strategy) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch10. 전략(Strategy) 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-04-24T01:00:00+09:00  
---  

## 01. Strategy
- 전략
	- 적과 싸울 때의 책략
	- 군대를 움직일 때의 작전
	- 문제를 해결해 나갈 때의 방법
	- 프로그래밍에서는 '알고리즘'  
- Strategy 패턴
	- 알고리즘을 구현한 부분이 모두 교환 가능하도록 함
	- 알고리즘(전략, 작전, 책략)을 교체해서 동일한 문제를 다른 방법으로 해결하는 패턴


## 02. 예제 프로그램

- 컴퓨터로 '가위바위보'하는 게임
- 두 가지 전략
	- WinningStrategy: 이기면 다음 번에도 같은 손을 내민다
	- ProbStrategy: 바로 전에 내밀었던 손에서 다음에 내밀 손을 확률적으로 계산한다  


### 클래스와 인터페이스

|이름|해설|
|--------|--------------|
|Hand|가위바위보의 '손'을 나타내는 클래스|
|Strategy|가위바위보의 '전략'을 나타내는 인터페이스|
|WinningStrategy|이기면 다음에도 같은 손을 내미는 전략을 나타내는 클래스|
|ProbStrategy|바로 전의 손에서 다음 손을 확률적으로 계산하는 전략을 나타내는 클래스|
|Player|가위바위보를 하는 플레이어를 나타내는 클래스|
|Main|동작 테스트용 클래스|


### 클래스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-1.png){: width="100%"}

### Hand 클래스
- 가위바위보의 손을 나타내는 클래스
- (정적)hand 필드: 가위 손, 주먹 손, 보 손 세 개의 손을 가지고 있는 배열
- handvalue 필드: 주먹은 0, 가위는 1, 보는 2로 표현
- getHand()
	- 가위바위보를 나타내는 숫자로부터 해당 손을 반환함
- isStrongerThan()
	- 현재 손이 입력 인자로 들어온 손을 이기면 true를 반환함
- isWeakerThan()
	- 현재 손이 입력 인자로 들어온 손에게 지면 true를 반환함
- fight()
	- 현재 손이 입력인자 손과 무승부면 0, 이기면 1, 지면 -1을 반환함
	- 우열 판정하는 수식  
	`(this.handvalue + 1) % 3 == h.handvalue`
		- 현재, 손이 주먹(0)이고 입력 손이 가위(1)라면
		- 또는, 현재 손이 가위(1)이고 입력 손이 보(2)라면
		- 또는, 현재 손이 보(2)이고, 입력 손이 주먹(0)이라면
		- 현재 손이 이긴다 ⇒ 1을 반환한다
		- 코드에서 this를 생략해도 된다  
		`(handvalue + 1) % 3 == h.handvalue`  
- toString(): 현재 손에 해당하는 문자열을 반환한다  

```java
public class Hand {
  public static final int HANDVALUE_GUU = 0; // 주먹을 표시하는 값
  public static final int HANDVALUE_CHO = 1; // 가위를 표시하는 값
  public static final int HANDVALUE_PAA = 2; // 보를 표시하는 값
  public static final Hand[] hand = { // 가위바위보의 손을 표시하는 3개의 인스턴스
    new Hand(HANDVALUE_GUU),
	new Hand(HANDVALUE_CHO),
	new Hand(HANDVALUE_PAA),
  };
  private static final String[] name = { // 가위바위보의 손의 문자열 표현
    "주먹", "가위", "보",
  };
  private int handvalue; // 가위바위보의 손의 값
  private Hand(int handvalue) {
    this.handvalue = handvalue;
  }
  public static Hand getHand(int handvalue) { // 값에서 인스턴스를 얻는다
    return hand[handvalue];
  }
  public boolean isStrongerThan(Hand h) { // this가 h에게 이길경우 true
    return fight(h) == 1;
  }
  public boolean isWeakerThan(Hand h) { // this가 h에게 질경우 true
    return fight(h) == -1;
  }
  private int fight(Hand h) { // 무승부는 0, this의 승이면 1, h의 승이면 -1
    if (this == h) {
      return 0;
    } else if ((this.handvalue + 1) % 3 == h.handvalue) {
      return 1;
    } else {
      return -1;
    }
  }
  public String toString() { // 문자열 표현으로 변환
    return name[handvalue];
  }
}
```


### Strategy 인터페이스
- 가위바위보의 '전략'을 위한 추상 메소드를 모아놓은 곳
- nextHand()
	- 다음에 내밀 손을 얻기 위해 호출하는 메소드
	- 이 메소드가 호출되면, Strategy 인터페이스를 구현한 클래스가 지혜를 모아 '다음 손'을 결정함
	- study()
		- 전략을 위한 준비를 하는 메소드
		- 이긴 경우에는 Player가 strudy(true)를 호출하고,
		- 진 경우에는 Player가 study(flase)를 호출한다

```java
public interface Strategy {
	public abstract Hand nextHand();
	public abstract void study(boolean win);
}
```


### WinningStrategy 클래스
- Strategy 인터페이스를 구현한 클래스
- nextHand()에서의 전략
	- 직전의 승부에서 승리했으면, 동일한 손을 내민다
	- 직전 승부에서 패했으면, 난수를 사용해서 다음 손을 정한다
		- java.util.Random 클래스 이용
			- nextInt(3): 0 부터 2 사이의 난수 정수 생성
	- 어리석은 전략이다
- won 필드: 지난번 승부에서 이겼으면 true, 졌으면 false 저장
- prevHand 필드: 지난번 승부에서 내민 손 저장

```java
import java.util.Random;

public class WinningStrategy implements Strategy {
	private Random random;
	private boolean won = false;
	private Hand prevHand;
	public WinningStrategy(int seed) {
		random = new Random(seed);
	}
	public Hand nextHand() {
		if (!won) {
			prevHand = Hand.getHand(random.nextInt(3));
		}
		return prevHand;
	}
	public void study(boolean win) {
		won = win;
	}
}
```

### ProbStrategy 클래스
- 좀 더 머리를 쓰는 전략
- history 필드: 과거의 승패를 유지하는 테이블
	- history[이전에 낸 손][이번에 낼 손]
	- 예
		- history[0][0]: 주먹, 주먹 순으로 손을 내밀어서 이긴 횟수
		- history[0][1]: 주먹, 가위 순으로 손을 내밀어서 이긴 횟수
		- history[0][2]: 주먹, 보 순으로 손을 내밀어서 이긴 횟수  
- prevHandValue 필드: 지난번에 냈던 손
- currentHandValue 필드: 이번에 냈던 손
- nextHand(): 다음에 낼 손을 반환함
	- handValue: 다음에 낼 손의 값을 저장함
	- 전략
		- 이전에 주먹을 냈더라면, history[0][0], history[0][1], history[0][2]로부터 다음에 낼 손의 확률을 계산하려고 한다
		- history[0][0] = 3, history[0][1] = 5, history[0][2] = 7이 있었다면,
			- 세 숫자를 다 더해서(3+5+7=15) 그 값을 seed로 해서 난수를 얻음
			- 난수가 0부터 3미만이라면, 주먹을 내고
			- 난수가 3이상 8미만이라면, 가위를 내고
			- 난수가 8이상 15미만이라면, 보를 낸다
- study(): 전략을 위한 준비 작버을 하는 메소드
	- 이번에 이겼으면,
		- history[직전에 냈던 손][이번에 냈던 손]에 1을 더한다
	- 이번에 졌으면,
		- history[직전에 냈던 손][이번에 안 냈던 손] 각각에 1을 더한다


```java
import java.util.Random;

public class ProbStrategy implements Strategy {
	private Random random;
	private int prevHandValue = 0;
	private int currentHandValue = 0;
	private int[][] history = {
		{ 1, 1, 1, },
		{ 1, 1, 1, },
		{ 1, 1, 1, },
		{ 1, 1, 1, },
	};
	public ProbStrategy(int seed) {
		random = new Random(seed);
	}
	public Hand nextHand() {
		int bet = random.nextInt(getSum(currentHandValue));
		int handvalue = 0;
		if (bet < history[currentHandValue][0] {
			handvalue = 0;
		} else if (bet < history[currentHandValue][0] + history[currentHandValue][1]) {
			handvalue = 1;
		} else {
			handvalue = 2;
		}
		preHandValue = currentHandValue;
		currentHandValue = handvalue;
		return Hand.getHand(handvalue);
	}
	private int getSum(int hv) {
		int sum = 0;
		for (int i = 0; i < 3; i++) {
			sum += history[hv][i];
		}
		return sum;
	}
	public void study(boolean win) {
		if (win) {
			history[prevHandValue][currentHandValue]++;
		} else {
			history[prevHandValue][(currentHandValue + 1) % 3]++;
			history[prevHandValue][(currentHandValue + 2) % 3]++;
		}
	}
}
```

### Player 클래스
- 가위바위보를 하는 사람을 표현한 클래스
- 생성 시, '이름'과 '전략'이 주어진다
- 생성 시의 '전략'에 따라 다음에 내밀 손이 결정된다
	- nextHand() 메소드 안에서 strategy의 nextHand()를 호출한다
	- Strategy에게 위임한다
- 이기든(win) 지든(lose) 무승부이든(even), 다음 승부를 위해서 strategy의 study() 메소드를 호출한다
	- win(), lose(), even()
- 승패 횟수를 저장
	- wincount, losecount, gamecount 필드

```java
public class Player {
	private String name;
	private Strategy strategy;
	private int wincount;
	private int losecount;
	private int gamecount;
	public Player(String name, Strategy strategy) { // 이름, 전략 할당받음
		this.name = name;
		this.strategy = strategy;
	}
	public Hand nextHand() { // 전략의 지시를 받음
		return strategy.nextHand();
	}
	public void win() {
		strategy.study(true);
		wincount++;
		gamecount++;
	}
	public void lose() {
		strategy.study(false);
		losecount++;
		gamecount++;
	}
	public void even() {
		gamecount++;
	}
	public String toString() {
		return "[" + name + ":" + gamecount + "games, " + wincount + "win, " + losecount + "lose" + "]";
	}
}
```


### Main 클래스
- 실제로 Player 두 명을 생성해서 가위바위보 게임을 시킴  
![](https://eliotjang.github.io/assets/images/system-analysis/ch10-2.png){: width="100%"}  

```java
public class Main {
  public static void main(String[] args) {
	if (args.length != 2) {
	  System.out.println("Usage: java Main randomseed1 randomseed2");
	  System.out.println("Example: java Main 314 14");
	  System.exit(0);
	}
	int seed1 = Integer.parseInt(args[0]);
	int seed2 = Integer.parseInt(args[1]);
	Player player1 = new Player("두리", new WinningStrategy(seed1));
	Player player2 = new Player("하나", new ProbStrategy(seed2));
	for (int i = 0; i < 10000; i++) {
	  Hand nextHand1 = player1.nextHand();
	  Hand nextHand2 = player2.nextHand();
	  if (nextHand1.isStrongerThan(nextHand2)) {
		System.out.println("Winner:" + player1);
		player1.win();
		player2.lose();
	  } else if (nextHand2.isStrongerThan(nextHand)) {
		System.out.println("Winner:" + player2);
		player1.lose();
		player2.win();
	  } else {
		System.out.println("Even...");
		player1.even();
		player2.even();
	  }
	}
	System.out.println("Total result:");
	System.out.println(player1.toString());
	System.out.println(player2.toString());
  }
}
```



![](https://eliotjang.github.io/assets/images/system-analysis/ch10-3.png){: width="100%"}


## 03. 등장 역할

- Strategy(전략)의 역할
	- Strategy 패턴을 이용하기 위한 인터페이스(API) 결정
	- 예제에서는 Strategy 인터페이스가 해당됨
- ConcreteStrategy(구체적 전략)의 역할
	- Strategy 인터페이스를 실제로 구현
	- 구체적 전략(작전, 책략, 알고리즘)을 나타냄
	- 예제에서는, WinningStrategy와 ProbStrategy가 해당됨
- Context(문맥)의 역할
	- Strategy를 이용하는 역할
	- ConcreteStrategy 인스턴스를 가지고, 필요에 따라서 이를 이용함
	- 예제에서는, Player가 해당됨


- 클래스 다이어그램  

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-4.png){: width="100%"}

## 04. 독자의 사고를 넓혀주는 힌트
### 일부러 Strategy 역할을 만들 필요가 있을까?
- Strategy 역학을 구현하기만 한다면, ConcreteStrategy의 종류를 변경하기가 쉽다
	- 예1: 예전의 알고리즘과 개량한 알고리즘의 속도를 비교하고 싶은 경우, 간단히 교체해서 테스트할 수 있다
	- 예2: 장기 게임에서 사용자의 선택에 따라 <u>사고 루틴의 레벨</u>을 교체하는 것도 간단하게 실행할 수 있다

### 실행 중에 교체하는 것도 가능하다
- 프로그램 동작 중에 ConcreteStrategy 역할을 교체할 수 있다
- 예: 메모리가 적은 환경에서는 SlowButLessMemoryStrategy를 사용하고, 메모리가 충분한 환경에서는 FastButMoreMemoryStrategy를 사용한다

## 05. 관련 패턴
- Flyweight 패턴(20장)
- Abstract factory 패턴(8장)
- State 패턴(19장)

## 06. 요약

- 알고리즘(전략)을 쉽게 교체할 수 있는 Strategy 패턴
	- 위임(delegation) 덕택에 가능함

## 연습 문제
- 각자 공부할 것
- <http://www.hyuki.com/dp/>  
![](https://eliotjang.github.io/assets/images/system-analysis/ch10-5.png){: width="70%"}

## 참고자료

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-6.png){: width="100%"}  

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-7.png){: width="100%"}  

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-8.png){: width="100%"}  

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-9.png){: width="100%"}  

![](https://eliotjang.github.io/assets/images/system-analysis/ch10-10.png){: width="100%"}






























