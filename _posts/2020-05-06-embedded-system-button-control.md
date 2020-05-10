---
title: "[임베디드시스템] Button 제어"
excerpt: "GWNU Embedded System Lecture"

toc: true
toc_sticky: true
toc_label: "Button 제어"

header:
  teaser: /assets/images/embedded-system/embedded-system-logo.jpeg

categories:
  - embedded system
tags:
  - embedded system
last_modified_at: 2020-05-06T21:00:00+09:00
---

## Button

- 회로 상에서 전류를 흐르게 하거나 차단하는 역할
- 핀의 극성은 없음
- 정격 고려하지 않아도 됨
    - 물론 전자부품이 감당하지 못하는 전류가 흐르면 탈수는 있지만 웬만하면 괜찮다


![](https://eliotjang.github.io/assets/images/embedded-system/button-control-1.png){: width="90%"}


## 버튼을 이용한 LED 제어

- thinkercad.com에 들어가면 회로구성 뿐만 아니라 코드를 집어넣어서 시뮬레이션까지 가능한 툴을 이용

![](https://eliotjang.github.io/assets/images/embedded-system/button-control-2.png){: width="80%"}


## 플로팅(Floating)

- 아무것도 연결되지 않은 상태를 일컫는 용어
- 작은 노이즈만으로 HIGH 또는 LOW 값을 가지기 때문에 아두이노의 오작동 유발 가능성 있음
- 플로팅을 방지하기 위해 사용되는 것이 풀업/풀다운

![](https://eliotjang.github.io/assets/images/embedded-system/button-control-3.png){: width="60%"}


## 풀업 회로

- 풀업 저항(10kΩ)을 사용한 회로
    - VCC(전원부)랑 저항이 연결되어 있음
- 아두이노에서 스위치를 연결할 때 사용되는 회로
- <span style="color:red">(우측사진)스위치를 누르지 않을 경우 5V, (좌측사진)누를 경우 0V</span>
    - 스위치를 누를 경우 전류가 오른쪽 INPUT으로 가는게 아니라서 INPUT이 0V가 된다. GND로 전류가 가는 이유는, 물은 높은 곳에서 아래쪽으로 가는 경우를 생각하면 편하다


![](https://eliotjang.github.io/assets/images/embedded-system/button-control-4.png){: width="70%"}


## 풀업 회로를 이용한 LED 제어

![](https://eliotjang.github.io/assets/images/embedded-system/button-control-5.png){: width="80%"}

```
void setup()
{
    pinMode(3, INPUT);
    pinMode(7, OUTPUT);
}
void loop()
{
    if (digitalRead(3) == LOW) {
	digitalWrite(7, HIGH);
    }
    else {
	digitalWrite(7, LOW);
    }
    delay(10);
}
```


## 풀다운 회로

- 접지부랑 저항이 연결되어 있음
- 풀업 회로와 반대로 0V 쪽에 저항 연결
- 스위치를 누르지 않을 경우 0V, 누를 경우 5V

![](https://eliotjang.github.io/assets/images/embedded-system/button-control-6.png){: width="70%"}


## 풀다운 회로를 이용한 LED 제어

![](https://eliotjang.github.io/assets/images/embedded-system/button-control-7.png){: width="80%"}

```
void setup()
{
    pinMode(3, INPUT);
    pinMode(7, OUTPUT);
}
void loop()
{
    if (digitalRead(3) == HIGH) {
	digitalWrite(7, HIGH);
    }
    else {
	digitalWrite(7, LOW);
    }
    delay(10);
}
```

## 내부 풀업

- 아두이노의 각 핀 내부의 풀업 저항
    - 풀업이니까 VCC(전원부)랑 저항이 연결되어 있음
    
- pinMode(pin_num, INPUT_PULLUP) 함수를 통해 사용

![](https://eliotjang.github.io/assets/images/embedded-system/button-control-8.png){: width="60%"}


## 내부풀업 회로를 이용한 LED 제어

![](https://eliotjang.github.io/assets/images/embedded-system/button-control-9.png){: width="80%"}

```
void setup()
{
    pinMode(3, INPUT_PULLUP);
    pinMode(7, OUTPUT);
}
void loop()
{
    if (digitalRead(3) == LOW) {
	digitalWrite(7, HIGH);
    }
    else {
	digitalWrite(7, LOW);
    }
    delay(10);
}
```

## 도전 과제: 토글 제어

- 현재 내부 풀업 회로에 사용되는 코드를 이용

- 2가지 상태를 번갈아가며 제어하는 방식
- 버튼을 누를 때마다 LED ON/OFF 토글


























