---
title: "[임베디드 시스템] 전자회로 기초"
excerpt: "GWNU Embedded System Lecture"

toc: true
toc_sticky: true
toc_label: "전자회로 기초"

header:
  teaser: /assets/images/embedded-system/embedded-system-logo.jpeg

categories:
  - 컴퓨터공학
tags:
  - 임베디드-시스템
last_modified_at: 2020-04-16T12:00:00+09:00
---

## 전압/전류/저항
- 전압 (Voltage)
	- 전기적 압력
	- 전기적 위치에너지: 에너지를 보유하고 있지만 쓰지않고 있음
	- 단위: 볼트(V)
- 전류 (Intensity of Current)
	- 전기적인 흐름, 운동 에너지
	- 단위: 암페어(mA)
	- 전압이 셀수록 전류도 세짐
- 저항 (Resistance)
	- 전류의 세기를 방해하는 요소
	- 단위: 옴(Ω)
	- 도체는 저항이 작고 부도체는 저항이 매우 큼

## 옴의 법칙
- 전압, 전류, 저항 사이의 관계를 식으로 표현
- 전압이 셀수록 전류는 강해지고 저항이 클수록 전류는 약해짐  

![](https://eliotjang.github.io/assets/images/embedded-system/electronic-circuit-1.png){: center-align width="80%"}

## 전원/접지
- 전원(+)
	- 회로에 전압을 공급
	- 배터리의 경우 (+)극이 전원
	- VCC, VDD 등으로 표기  
	![](https://eliotjang.github.io/assets/images/embedded-system/electronic-circuit-2.png){: center-align width="40%"}
- 접지(-)
	- 전압의 크기 결정
	- 배터리의 경우 (-)극이 접지
	- GND(ground)로 표기
	- 모든 회로는 같은 접지를 사용해야 함  
	![](https://eliotjang.github.io/assets/images/embedded-system/electronic-circuit-3.png){: center-align width="60%"}  

## 정격 전압 및 전류
- 정격 전압: 적합한 전압
- 정격 전류: 적합한 전류
- 정격 이상의 전압 혹은 전류가 흐르면 전자부품이 망가짐
- 정격 출력 (W): 전압 X 전류

## 핀맵
- 핀은 회로를 연결하는 지점
- 각각의 핀은 용도가 정해져 있음
- 핀 별로 용도를 정해놓은 것을 핀맵이라고 함  

![](https://eliotjang.github.io/assets/images/embedded-system/electronic-circuit-4.png){: center-align width="80%"}

## 아두이노 우노 핀맵  

![](https://eliotjang.github.io/assets/images/embedded-system/electronic-circuit-5.png){: center-align width="70%"}

## 저항 읽는 방법  

![](https://eliotjang.github.io/assets/images/embedded-system/electronic-circuit-6.png){: center-align width="100%"}

## 아두이노 입출력핀
- 출력핀
	- 전원 역할을 하는 핀
	- 전압이 걸리고 전류가 흘러 나감
- 입력핀 (센서 역할)
	- 전압을 측정하는 핀
	- 전류가 흘러 들어와 전압이 걸림

## 디지털 vs 아날로그
- 디지털
	- OV와 5V만 사용
	- 이외 다른 전압은 인식 못함
- 아날로그
	- 0V ~ 5V까지 모두 사용  
	![](https://eliotjang.github.io/assets/images/embedded-system/electronic-circuit-7.png){: center-align width="60%"}

## 회로
- 전자 부품들이 서로 연결되어 있는 것
- 전원(VCC)과 접지(GND)가 반드시 존재해야 함
- 전원에서 접지까지 연결되어야 함  
![](https://eliotjang.github.io/assets/images/embedded-system/electronic-circuit-8.png){: center-align width="60%"}
















