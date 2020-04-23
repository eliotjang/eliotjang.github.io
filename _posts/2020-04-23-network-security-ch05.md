---
title: "[네트워크보안] Chapter 05. 데이터베이스(DB) 보안"
excerpt: "장상수 저, 정보보호총론, 생능출판, 2015"
toc: true
toc_sticky: true
toc_label: "Ch05. 데이터베이스(DB) 보안"
header:
  teaser: /assets/images/network-security/nerwork-security-logo.jpeg
categories:
  - network security
tags:
  - network security
last_modified_at: 2020-04-23T16:00:00+09:00
---  

## 5.1 DB 보안 개요
### 1. DB(Database) 보안의 개념

### 2. DB 보안 요구사항
1. 권한을 부여받은 사용자만 접근 허용

2. 추론방지

3. DB의 무결성 보장

4. 데이터의 논리적 일관성 보장

5. 감사 기능

6. 사용자 인증

7. 제한(confinement)

### 3. 데이터 무결성 규칙의 종류

1. 도메인 무결성 규칙(Domain Integrity Rule)

2. 개체 무결성 규칙(Entity Integrity Rule)

3. 참조 무결성 규칙(Referentail Integrity Rules)

![](https://eliotjang.github.io/assets/images/network-security/ch05-1.png){: width="100%"}

4. 스키마(Schema)

	- ①개념 스키마(Conceptual Schema):
	- ②외부 스키마(External Schema):
	- ③내부 스키마(Internal Schema):

5. SQL(Structured Query Language)
	1. 개념

	2. 명령어의 종류

		- ①데이터 정의 언어(DDL, Data Definition Language):
		- ②데이터 조작 언어(DML, Data Manipulation Language):
		- ③데이터 제어 언어(DCL, Data Control Language):


## 5.2 DB 보안 주요 위협

1. 일반적인 DB 위협

2. 사용자에 대한 식별 및 인증 위협

3. 데이터 유출 위협

4. 권한 오·남용 위협

5. 암호 모듈 오용 위협

6. 암·복호화키 및 마스터 키 노출 위협

7. 프로젝트 수행과정에서의 DB 접근 허용 위협

8. 운영과정에서의 권한 관리 소홀로 인한 침해 발생 위협

9. 접근통제 우회

## 5.3 DB 접근제어 및 암호화

### 1. DB 접근 통제

1. 에이전트 방식

![](https://eliotjang.github.io/assets/images/network-security/ch05-2.png){: width="80%"}

2. 게이트웨이 방식


![](https://eliotjang.github.io/assets/images/network-security/ch05-3.png){: width="100%"}

3. 스니핑 방식


![](https://eliotjang.github.io/assets/images/network-security/ch05-4.png){: width="100%"}


### 2. DB 암호화

![](https://eliotjang.github.io/assets/images/network-security/ch05-5.png){: width="100%"}

1. TDE(Transparent Data Encryption) 방식

2. 파일 암호화 방식

![](https://eliotjang.github.io/assets/images/network-security/ch05-6.png){: width="100%"}

### 3. DB 마스킹(Masking)

![](https://eliotjang.github.io/assets/images/network-security/ch05-7.png){: width="100%"}















