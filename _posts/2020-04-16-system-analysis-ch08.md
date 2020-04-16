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
			<td>추상적인 공장을 나타내는 클래스(Link, Tray, Page를 만듬)</td>
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
![](https://eliotjang.github.io/assets/images/system-analysis/ch08-2.png){: width="100%"}  

### 디렉토리 구조
![](https://eliotjang.github.io/assets/images/system-analysis/ch08-3.png){: width="100%"}  

### 추상적인 부품: Item 클래스
- Link와 Tray의 상위 클래스
	- Link나 Tray를 동일시 하기 위한 클래스
- caption 필드
	- 목차를 담는 변수
- makeHTML()
	- HTML의 문자열을 반홤함
	- 하위 클래스에서 구현되도록 기대됨  

```java
package factory;

public abstract class Item {
	protected String caption;
	public Item(String caption) {
		this.caption = caption;
	}
	public abstract String makeHTML();
}
```  

### 추상적인 부품: Link 클래스
- HTML의 하이퍼링크를 추상적으로 표현한 클래스
- url 필드
	- URL을 저장하는 변수
- 상위 클래스 Item에서 상속받은 makeHTML() 메소드가 <span style="color:red">구현되어 있지 않으므로</span>, 추상 클래스이다  

```java
package factory;

public abstract class Link extends Item {
	protected String url;
	public Link(String caption, String url) {
		super(caption);
		this.url = url;
	}
}
```  

### 추상적인 부품: Tray 클래스
![](https://eliotjang.github.io/assets/images/system-analysis/ch08-4.png){: align-center width="30%"}

- 다수의 Link나 Tray를 모아 집합으로 만드는 클래스
- tray 필드
	- ArrayList 객체 이용
- add()
	- 항목을 추가할 때 호출되는 메소드
	- Link와 Tray의 상위 클래스인 Item을 인수로 가짐
- 상위 클래스 Item에서 상속받은 makeHTML() 메소드가 <span style="color:red">구현되어 있지 않으므로</span>, 추상 클래스이다  

```java
package factory;
import java.util.ArrayList;

public abstract class Tray extends Item {
	protected ArrayList tray = new ArrayList();
	public Tray(String caption) {
		super(caption);
	}
	public void add(Item item) {
		try.add(item);
	}
}
```  

### 추상적인 제품: Page 클래스
- HTML 페이지 전체를 추상적으로 표현한 클래스
- Link, Tray는 부품, Page는 제품을 나타낸다
- title, author 필드
- add()
	- Item(Link나 Tray)을 추가할 때 사용되는 메소드
- output()
	- makeHTML()의 결과를 파일에 쓴다  
	`writer.write(this.makeHTML());`  
	- 간단한 Template Method 패턴  

```java
package factory;
import java.io.*;
import java.util.ArrayList;

public abstract class Page {
	protected String title;
	protected String author;
	protected ArrayList content = new ArrayList();
	public Page(String title, String author) {
		this.title = title;
		this.author = author;
	}
	public void add(Item item) {
		content.add(item);
	}
	public void output() {
		try {
			string filename = title + ".html";
			Writer writer = new FileWriter(filename);
			writer.write(this.makeHTML());
			writer.close();
			System.out.println(filename + "을 작성했습니다.");
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	public abstract String makeHTML();
}
```  

### 추상적인 공장: Factory 클래스
- getFactory()
	- 구체적인 공장을 나타내는 클래스명을 인자로 받아서, 그 클래스의 객체를 반환하는 메소드  
	`factory = (Factory)Class.forName(classname).newInstance();`  
	- <span style="color:red">Class 클래스의 forName 메소드를 사용해서, 그 클래스를 동적으로 읽음. 그리고 newInstance 메소드를 이용해서, 그 클래스의 인스턴스를 한 개 작성</span>
	- 이 메소드의 반환 타입은, Factory(즉, 구체적인 공장의 상위클래스) 추상클래스이다  

```java
package factory;

public abstract class Factory {
	public static Factory getFactory(String classname) {
		Factory factory = null;
		try {
			factory = (Factory)Class.forName(classname).newInstance();
		} catch (ClassNotFoundException e) {
			System.err.println("클래스" + classname + "이 발견되지 않습니다.");
		} catch (Exception e) {
			e.printStackTrace();
		}
		return factory;
	}
	public abstract Link createLink(String caption, String url);
	public abstract Tray createTray(String caption);
	public abstract Page createPage(String title, String author);
}
```  

### 공장을 사용해서 부품을 조립하여 제품을 만든다: Main 클래스
- import되어 있는 것은, factory 패키지 뿐이다
	- 즉, 구체적인 공장은 전혀 사용하지 않는다
- 구체적인 공장의 클래스명은, 커맨드 라인 인자로 지정한다
	- `c:> java Main listfactory.ListFactory`
		- `listfactory.ListFactory`가 args[0]으로 전달된다
	- args[0]와 Factory.getFactory를 이용하여 구체적인 공장의 인스턴스를 얻어온다  
	`Factory factory = Factory.getFactory(args[0]);`
- 공장의 메소드를 이용하여 부품과 제품을 생산한다  

```java
import factory.*;

public class Main {
	public static void main(String[] args) {
		if (args.length != 1) {
			System.out.println("Usage: java Main clas.name.of.ConcreteFactory");
			System.out.println("Example 1: java Main listfactory.ListFactory");
			System.out.println("Example 2: java Main tablefactory.TableFactory");
			System.exit(0);
		}
		Factory factory = Factory.getFactory(args[0]);

		Link joins = factory.createLink("중앙일보", "http://
```


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


































