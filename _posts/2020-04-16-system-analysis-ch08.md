---
title: "[시스템분석및설계] Chapter 8. 추상적(Abstract) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch8. 추상적(Abstract) 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-04-16T18:00:00+09:00  
---  

## 01. Abstract Factory 패턴
- '추상적(abstract)'
	- 구체적으로 어떻게 구현되는지는 생각하지 않고 인터페이스(API)만을 주목하고 있는 상태
	- 예: 추상 메소드(abstract method)
		- 메소드의 본체는 쓰여저 있지 않고, 이름과 시그너처(인자의 형과 갯구, 반환형)만 정해져 있는 메소드
- 추상적인 공장에서 추상적인 부품을 조립하여 추상적인 제품을 만든다
	- 하위 클래스에서 구체적인 구현을 수행한다

## 02. 예제 프로그램
- 계층 구조를 가진 Link 페이지를 HTML 파일로 만드는 프로그램
- 세 가지 패키지
	- factory 패키지: 추상적인 공장, 부품, 제품을 포함한 패키지
	- 이름 없는 패키지: Main 클래스를 포함한 패키지
	- listfactory 패키지: 구체적인 공장, 부품, 제품을 포함한 패키지  

![](https://eliotjang.github.io/assets/images/system-analysis/ch08-1.png){: width="100%"}  

<table>
	<thead>
		<tr>
			<th>패키지</th>
			<th>이름</th>
			<th>해설</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td rowspan=5>factory</td>
			<td>Factory</td>
			<td>추상적인 공장을 나타내는 클래스(Link, Tray, Page를 만듬)<td>
		</tr>
		<tr>
			<td>Item</td>
			<td>Link와 Tray를 통일적으로 다루기 위한 클래스</td>
		</tr>
		<tr>
			<td>Link</td>
			<td>추상적인 부품: HTML의 링크를 나타내는 클래스</td>
		</tr>
		<tr>
			<td>Tray</td>
			<td>추상적인 부품: Link와 Tray를 모은 클래스</td>
		</tr>
		<tr>
			<td>Page</td>
			<td>추상적인 제품: HTML의 페이지를 나타내는 클래스</td>
		</tr>
		<tr>
			<td>Anonymous</td>
			<td>Main</td>
			<td>동작 테스트용 클래스</td>
		</tr>
		<tr>
			<td rowspan=4>listfactory</td>
			<td>ListFactory</td>
			<td>구체적인 공장을 나타내는 클래스(ListLink, ListTray, ListPage를 만듬)</td>
		</tr>
		<tr>
			<td>ListLink</td>
			<td>구체적인 부품: HTML의 링크를 나타내는 클래스</td>
		</tr>
		<tr>
			<td>ListTray</td>
			<td>구체적인 부품: Link나 Tray를 모은 클래스</td>
		</tr>
		<tr>
			<td>ListPage</td>
			<td>구체적인 제품: HTML의 페이지를 나타내는 클래스</td>
		</tr>
	</tbody>
</table>  



### 클래스 다이어그램

### 디렉토리 구조

### 추상적인 부품: Item 클래스

### 추상적인 부품: Link 클래스

### 추상적인 부품: Tray 클래스

### 추상적인 제품: Page 클래스

### 추상적인 공장: Factory 클래스

### 구체적인 공장: ListFactory 클래스

### 구체적인 부품: ListLink 클래스

### 구체적인 부품: ListTray 클래스

### 구체적인 제품: ListPage 클래스

## 03. 예제 프로그램에 별도의 구체적인 공장을 추가

### 구체적인 공장: TableFactory 클래스

### 구체적인 부품: TableLink 클래스

### 구체적인 부품: TableTray 클래스

### 구체적인 제품: TablePage 클래스

## 04. 등장 역할

## 05. 독자의 사고를 넓혀주는 힌트

## 06. 관련 패턴

## 07. 보강: 인스턴스를 만드는 여러 가지 방법

## 08. 요약

## 연습문제


































