---
title: "[임베디드 시스템] Sensor 제어"
excerpt: "GWNU Embedded System Lecture"

toc: true
toc_sticky: true
toc_label: "Sensor 제어"

header:
  teaser: /assets/images/embedded-system/embedded-system-logo.jpeg

categories:
  - 컴퓨터공학
tags:
  - 임베디드-시스템
last_modified_at: 2020-05-06T21:00:00+09:00
---

## 조도 센서

- 빛의 밝기를 측정할 수 있는 센서
- 포토레지스터, CDS 등으로 불림
- 핀 극성 없음
- 정격전압: 4 ~ 6V

![](https://eliotjang.github.io/assets/images/embedded-system/sensor-control-1.png){: width="40%"}

## 빛의 밝기에 따라 저항 값이 바뀜

|밝기(Lux)|저항(Ω)|V<sub>out</sum>(V)|
|-------|-------|--------|
|10,000|100|4.95|
|1,000|300|4.85|
|100|1,500|4.34|
|10|10,000|2.5|
|1|70,000|0.62|
|0.1|600,000|0.08|  

## 조도센서를 활용한 LED 제어

- 아날로그 입력은 0 ~ 1023까지 받고, 뿌려주는 값은 0 ~ 255이다
- 과전류를 방지하기 위해 조도센서 옆에 저항을 달아준다


![](https://eliotjang.github.io/assets/images/embedded-system/sensor-control-2.png){: width="80%"}

```
void setup()
{
    pinMode(A0, INPUT);
    pinMode(11, OUTPUT);
    Serial.begin(9600);
}
void loop()
{
    Serial.println(analogRead(A0));
    if(analogRead(A0) > 850) {
	digitalWrite(11, LOW);
    }
    else {
	digitalWrite(11, HIGH);
    }
    delay(100);
}
```

## 온도 센서(TMP36)

- 온도를 감지하여 전기적인 신호로 바궈주는 센서
- 온도에 따른 전압의 변화량을 이용하여 온도 측정

![](https://eliotjang.github.io/assets/images/embedded-system/sensor-control-3.png){: width="100%"}


## 온도센서를 이용한 LED 제어

![](https://eliotjang.github.io/assets/images/embedded-system/sensor-control-4.png){: width="80%"}


















