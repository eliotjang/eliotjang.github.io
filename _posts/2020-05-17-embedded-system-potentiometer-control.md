---
title: "[임베디드 시스템] Potentiometer 제어"
excerpt: "GWNU Embedded System Lecture"

toc: true
toc_sticky: true
toc_label: "Potentiometer 제어"

header:
  teaser: /assets/images/embedded-system/embedded-system-logo.jpeg

categories:
  - 컴퓨터공학
tags:
  - 임베디드 시스템
last_modified_at: 2020-05-17T00:31:00+09:00
---

## 가변저항 (Potentiometer)

![](https://eliotjang.github.io/assets/images/embedded-system/potentiometer-1.png){: width="70%"}

- 저항값의 변화가 가능한 전자 부품
- 조명의 밝기조절장치나 음향기기의 볼륨조절장치로 사용
- 정격은 고려하지 않음

## 가변저항을 활용한 LED 제어

![](https://eliotjang.github.io/assets/images/embedded-system/potentiometer-2.png){: width="100%"}

## 가변저항을 이용한 LED 제어 코드

```
void setup()
{
  pinMode(A0, INPUT);
  pinMode(11, OUTPUT);
  Serial.bigin(9600);
}
void loop()
{
  delay(10);
}
```





