---
title: "[임베디드 시스템] Piezo buzzer 제어"
excerpt: "GWNU Embedded System Lecture"

toc: true
toc_sticky: true
toc_label: "Piezo buzzer 제어"

header:
  teaser: /assets/images/embedded-system/embedded-system-logo.jpeg

categories:
  - 컴퓨터공학
tags:
  - 임베디드 시스템
last_modified_at: 2020-05-12T17:30:00+09:00
---

## 피에조 부저 (압전 스피커)

![](https://eliotjang.github.io/assets/images/embedded-system/piezo-buzzer-control-1.png){: width="40%"}

- 전기적 신호를 이용하여 소리를 내는 전자 부품
- 압전효과(피에조 효과): 물체에 압력을 주면 전기가 발생하는 현상
- 능동 부저와 수동 부저로 분류


## 피에조 부저를 활용한 회로

![](https://eliotjang.github.io/assets/images/embedded-system/piezo-buzzer-control-2.png){: width="80%"}


## 피에조 부저 회로 코드

```
void setup()
{
    pinMode(7, OUTPUT);
}
void loop()
{
    tone(7, 523, 100); // play tone 60 (도)
    delay(500);
    tone(7, 587, 100); // play tone 62 (레)
    delay(500);
    tone(7, 659, 100); // play tone 64 (미)
    delay(500);
}
```

## [실습] 버튼을 활용한 피에조 부저 회로

![](https://eliotjang.github.io/assets/images/embedded-system/piezo-buzzer-control-3.png){: width="80%"}













