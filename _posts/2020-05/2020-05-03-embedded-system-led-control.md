---
title: "[임베디드 시스템] LED 제어"
excerpt: "GWNU Embedded System Lecture"

toc: true
toc_sticky: true
toc_label: "LED 제어"

header:
  teaser: /assets/images/embedded-system/embedded-system-logo.jpeg

categories:
  - 컴퓨터공학
tags:
  - 임베디드-시스템
last_modified_at: 2020-05-03T12:18:00+09:00
---

## LED

- 전류가 흐르면 빛이 나는 전자부품
- 복습: 출력핀에 꽂아서 전압을 걸고 전류를 흐르게 한다
- 다리 길이가 긴 것이 양극, 짧은 것이 음극
- 정격: 0.1W = 5V * 0.02A(20mA)
    - 아두이노가 기본적으로 전압 5V를 사용하므로, 20mA이상의 세기의 전류를 흐르면 LED가 타게 되므로 저항이 필요하다
- 250 옴 이상의 저항 필요 (220 혹은 330 주로 사용)

![](https://eliotjang.github.io/assets/images/embedded-system/led-control-1.png){: width="70%"}

> <span style="color:green">출력핀하고 다리 길이가 긴 것에 연결을 시켜야 한다</span>
> <span style="color:green">만약 +,-의 다리길이가 같다면 어떻게 구별하느냐? LED안을 잘 살펴보고 넓게 보이는 부분이 음윽이고 상대적으로 좁은 부분이 +극이다.</span> 


## LED 회로 구현

- 2번 핀을 출력핀으로 쓴다
- 그라운드 핀으로 들어간다

![](https://eliotjang.github.io/assets/images/embedded-system/led-control-2.png){: width="90%"}

## LED 회로 완성

![](https://eliotjang.github.io/assets/images/embedded-system/led-control-3.png){: width="90%"}

## LED 회로 세부 화면

![](https://eliotjang.github.io/assets/images/embedded-system/led-control-4.png){: width="70%"}

## 아두이노 코드

- setup(): 초기화 과정을 수행

- pinMode()
    - 아두이노가 가지고 있는 입출력핀을 얘기한다. 아두이노는 디지털핀, 아날로그핀을 가지고 있는데, 핀마다 번호(이름)가 부여되어 있는데, 이 핀을 어떤 용도로 사용할 것인가에 대해서 setup()에 설정해줘야한다.
    - OUTPUT: 출력핀(전류를 흘러가게함), INPUT: 입력핀(센서의 역할)
    
- `pinMode(2, OUTPUT)` : 아두이노 2번 핀을 OUTPUT 핀으로 설정하겠다는 얘기
- `digitalWrite(2, HIGH)` : 2번핀에다가 전압 5V를 건다
- `digitalWrite(2, LOW)` : 2번핀에다가 전압 0V를 건다

![](https://eliotjang.github.io/assets/images/embedded-system/led-control-5.png){: width="90%"}














