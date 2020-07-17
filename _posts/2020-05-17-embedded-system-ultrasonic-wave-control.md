---
title: "[임베디드 시스템] Ultrasonic wave 제어"
excerpt: "GWNU Embedded System Lecture"

toc: true
toc_sticky: true
toc_label: "Ultrasonic wave 제어"

header:
  teaser: /assets/images/embedded-system/embedded-system-logo.jpeg

categories:
  - 컴퓨터공학
tags:
  - 임베디드 시스템
last_modified_at: 2020-05-17T00:30:00+09:00
---

## 초음파 센서(HC-SR04)

![](https://eliotjang.github.io/assets/images/embedded-system/ultrasonic-wave-control-1.png){: width="80%"}

- 초음파 센서 모듈
- 핀맵
  - VCC (5V)
  - GND
  - TRIG (Digital Output)
  - ECHO (Digital Input)
- 정격은 고려하지 않음

## 초음파 센서를 이용한 거리 측정 원리

- 초음파: 가청영역(20 ~ 20000 Hz)의 주파수보다 높은 음
- 초음파의 속도: 340 m/s
- 약 40KHz 주파수 생성 / 최대 4 ~ 5 m의 거리 측정
- 거리 측정 원리
  - 송신부(TRIG pin)에서 일정한 시간의 간격을 둔 초음파 펄스를 내보내고 물체에 부딪혀 돌아오는 에코 신호를 수신부(ECHO)에서 받아 시간차를 기반으로 거리 측

## HC-SR04 제어

- 초음파 발생
  - TRIG에 HIGH 출력
  - 짧은 시간 후 TRIG에 LOW 출력
- 초음파 수신: ECHO에 HIGH 출력
- 거리 계산
  - 거리(mm) = 측정시간(msec) x 0.23 / 2

## 초음파 센서를 이용한 거리 측정

![](https://eliotjang.github.io/assets/images/embedded-system/ultrasonic-wave-control-2.png){: width="100%"}

## 거리 측정 코드

```
void setup()
{
  pinMode(3, OUTPUT);
  pinMode(2, INPUT);
  Serial.bigin(9600);
}
void loop()
{
  digitalWrite(3, LOW);
  delayMicroseconds(4);
  digitalWrite(3, HIGH);
  delayMicroseconds(20);
  digitalWrite(3, LOW);

  unsigned long duration = pulseln(2, HIGH, 5000);
  float distance = 800; // 80cm
  if (duration > 0)
    distance = duration * 0.17;
  Serial.println(distance);
}
```

## [실습] 초음파센서로 자동차 후방 장애물 감지기 제작

![](https://eliotjang.github.io/assets/images/embedded-system/ultrasonic-wave-control-3.png){: width="100%"}









