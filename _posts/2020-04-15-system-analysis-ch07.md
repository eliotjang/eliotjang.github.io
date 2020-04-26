---
title: "[시스템분석및설계] Chapter 7. 빌더(Builder) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch7. 빌더(Builder) 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-04-15T18:00:00+09:00  
---  

## 01. Builder 패턴
- 구조를 가진 인스턴스를 쌓아 올리는 패턴
- 복잡한 객체를 생성하는 방법과 표현하는 방법을 정의하는 클래스를 별도로 분리하여 서로 다른 표현이라도 이를 생성할 수 있는 동일한 구축 공정 제공

## 02. 예제 프로그램
- Builder 패턴을 사용해서, '문서'를 작성하는 프로그램
- '문서'의 구조
	- 타이틀을 하나 포함한다.
	- 문자열을 여러 개 포함한다.
	- 개별 항목을 여러 개 포함한다.
- Builder 클래스는 문서를 구성하기 위한 메소드를 제공하는 추상클래스이다.
	- Director 클래스가 이 메소드를 이용해서 구체적인 하나의 문서를 만든다.
	- 문서를 작성하기 위한 구체적인 처리는 Builder 클래스의 하위클래스가 결정한다.
		- TextBuilder 클래스: 일반 텍스트를 이용해서 문서를 만든다
		- HTMLBuilder 클래스: HTML을 사용해서 문서를 만든다
- Director가 TextBuilder를 사용하면, 일반텍스트의 문서가 생기고, HTMLBuilder를 사용하면 HTML 문서가 생긴다.  

|이름|해설|
|--------------|------------------------------------------------------------|
|Builder|문서를 구성하기 위한 메소드를 결정하는 추상 클래스|
|Director|하나의 문서를 만드는 클래스
|TextBuilder|일반 텍스트(보통의 문자열)를 사용해서 문서를 만드는 클래스|
|HTMLBuilder|HTML 파일을 사용해서 문서를 만드는 클래스|
|Main|동작 테스트용 클래스|  


### 클래스 다이어그램
![](https://eliotjang.github.io/assets/images/system-analysis/ch07-1.png){: width="100%"}

### Builder 클래스
- '문서'를 만들 때 사용되는 메소드들을 선언한 추상 클래스
	- makeTitle(), makeString(), makeItems()
- close()
	- 문서를 완성시키는 메소드  

```java
public abstract class Builder {
	public abstract void makeTitle(String title);
	public abstract void makeString(String str);
	public abstract void makeItems(String[] items);
	public abstract void close();
}
```  

### Director 클래스
- Builder 클래스에 선언되어 있는 메소드를 이용해서 문서를 만듬
- Director 클래스의 생성자의 인자는 Builder 형.
	- 실제로는, Builder 하위 클래스가 전달된다.
	- Builder는 추상클래스이기 때문
- construct(): 문서를 만드는 메소드
	- Builder에 선언된 메소드들을 이용하여 문서를 만든다.  

```java
public class Director {
	private Builder builder;
	public Director(Builder builder) { // Builder의 Subclass의 인스턴스가 주어지므로
		this.builder = builder; // builder 필드에 저장
	}
	public void construct() { // 문서구축
		builder.makeTitle("Greeting"); // 타이틀
		builder.makeString("아침과 낮에"); // 문자열
		builder.makeItems(new String[] { // 개별항목
			"좋은 이침입니다",
			"안녕하세요",
		});
		builder.makeString("밤에"); // 별도의 문자열
		builder.makeItems(new String[]{ // 별도의 개별항목
			"안녕하세요",
			"안녕히 주무세요",
			"안녕히 계세요",
		});
		builder.close(); // 문서 완성
	}
}
```  

### TextBuilder 클래스
- Builder 클래스의 하위 클래스
- StringBuffer를 이용하여, 일반 텍스트 문서를 만든다.  

```java
public class TextBuilder extends Builder {
	private StringBuffer buffer = new StringBuffer(); // 필드의 문서 구축
	public void makeTitle(String title) { // 일반 텍스트의 제목
		buffer.append("===============================\n"); // 장식선
		buffer.append( "『" + title + "』\n");
		buffer.append("\n"); // 빈행
	}
	public void makeString(String str); { // 일반 텍스트에서의 문자열
		buffer.append("■" + str + "\n"); // ■글머리 기호가 붙은 문자열
		buffer.append("\n"); // 빈행
	}
	public void makeItems(String[] items) { // 일반 텍스트에서의 개별항목
		for (int i = 0; i < items.length; i++) {
			buffer.append("•" + items[i] + "\n"); // ·글머리 기호가 붙은 문자열
		}
		buffer.append("\n"); // 빈행
	}
	public void close() { // 문서의 완성
		buffer.append("================================\n"); // 장식선
	}
	public String getResult() { // 완성한 문서
		return buffer.toString(); // StringBuffer을 String으로 변환
	}
}
```  

### HTMLBuilder 클래스
- Builder 클래스의 하위 클래스
- PrintWriter를 이용하여, HTML 태그가 포함된 HTML 문서 파일을 만든다.  

```java
import java.io.*;

public class HTMLBuilder extends Builder {
	private String filename; // 작성할 파일명
	private PrintWriter writer; // 파일에 쓸 PrintWriter
	public void makeTitle(String title) { // HTML 파일의 타이틀
		filename = title + ".html"; // 타이틀을 기초로 파일명을 결정
		try {
			writer = new PrintWriter(new FileWriter(filename)); // PrintWriter을 만든다
		} catch (IOException e) {
			e.printStackTrace();
		}
		write.println("<html><head><title>" + title + "</title></head><body>"); // 타이틀 출력
		write.println("<h1>" + title + "</h1>");
	}
	public void makeString(String str) { // HTML 파일에서의 문자열
		writer.println("<p>" + str + "</p>"); // <p> 태그로 출력
	}
	public void makeItems(String[] items) { // HTML 파일에서의 개별항목
		writer.println("<ul>"); // <ul>과 <li>로 출력
		for (int i = 0; i < items.length; i++) {
			write.println("<li>" + items[i] + "</li>");
		}
		writer.println("</ul>");
	}
	public void close() { // 문서의 완성
		writer.println("</body></html>"); // 태그 닫기
		writer.close(); // 파일 닫기
	}
	public String getResult() {
		return filename; // 파일명 반환
	}
}
```  

### Main 클래스
- Main은 Director의 생성자를 이용해서 필요한 구체적인 Builder를 포함하는 Director를 생성한다.
	- 커맨드 라인에서 지정한 형식에 따른 문서를 만든다.
		- plain을 지정한 경우에는 TextBuilder 클래스의 인스턴스를 Director 클래스의 생성자에 전달.
		- html을 지정한 경우에는 HTMLBuilder 클래스의 인스턴스를 Director 클래스의 생성자에 전달
- Director는 Builder의 메소드만을 사용해서 문서를 만든다.
	- Director는 실제로 작동하고 있는 것이 TextBuilder인지, HTMLBuilder인지 의식하지 못한다.  

```java
public class Main {
	public static void main(String[] args) {
		if (args.length != 1) {
			usage();
			System.exit(0);
		}
		if (args[0].equals("plain")) {
			TextBuilder textbuilder = new TextBuilder();
			Director director = new Director(textbuilder);
			director.construct();
			String result = textbuilder.getResult();
			System.out.println(result);
		} else if (args[0].equals("html")) {
			HTMLBuilder htmlbuilder = new HTMLBuilder();
			Director director = new Director(htmlbuilder);
			director.construct();
			String filename = htmlbuilder.getResult();
			System.out.println(filename + "가 작성되었습니다.");
		} else {
			usage();
			System.exit(0);
		}
	}
	public static void usage() {
		System.out.println("Usage: java Main plain 일반텍스트로 문서작성");
		System.out.println("Usage: java Main html HTML 파일로 문서작성");
	}
}
```  

![](https://eliotjang.github.io/assets/images/system-analysis/ch07-2.png){: width="70%"}  

![](https://eliotjang.github.io/assets/images/system-analysis/ch07-3.png){: width="100%"}  


## 03. 등장 역할
- Builder(건축자)의 역할
	- 인스턴스 구축을 위한 인터페이스(API)를 결정한다
	- 예제에서는, Builder 클래스가 해당됨
- ConcreteBuilder(구체적 건축자)의 역할
	- Builder 인터페이스를 구현한 구체적인 클래스
	- 예제에서는, TextBuilder나 HTMLBuilder가 해당됨
- Director(감독자)의 역할
	- Builder의 인터페이스만을 사용해서, 인스턴스를 생성한다
	- ConcreteBuilder에 의존한 프로그래밍은 하지 않는다
- Client(의뢰인)의 역할
	- Builder 패턴을 이용하는 역할
	- 예제에서, Main 클래스가 해당됨
- 클래스 다이어그램  

![](https://eliotjang.github.io/assets/images/system-analysis/ch07-4.png){: width="90%"}
- 시퀀스 다이어그램  

![](https://eliotjang.github.io/assets/images/system-analysis/ch07-5.png){: width="60%"}


## 04. 독자의 사고를 넓혀주는 힌트
- 클래스가 알고 있는 것은 무엇인가?
	- Director 클래스는 자신이 이용하고 있는 Builder 클래스의 하위클래스가 무엇인지 모른다.
		- 이유: Director 클래스 안에서는, TextBuilder나 HTMLBuilder 클래스는 전혀 사용하지 않고, Builder 만을 이용하기 때문
	- 모르기 때문에, <span style="color:red">부품 교환</span>이 용이하다
		- 예1: 새로운 구체적인 Builder를 추가하기가 쉽다
			- XMLBuilder를 쉽게 여기에 추가할 수 있다
		- 예2: 기존의 TextBuilder나 HTMLBuilder의 수정이 용이하다


## 05. 관련 패턴
- Template Method(3장)
	- Builder 패턴에서는 Director가 Builder를 제어한다
	- Template Method 패턴에서는 상위클래스가 하위클래스를 제어한다
- Composite(11장)
- Facade 패턴(15장)


## 06. 요약
- 구조를 가진 인스턴스를 쌓아올리는 Builder 패턴


## 07. 연습 문제
- 7-1
	- Builder 추상클래스를 인터페이스로 바꾸었을 때 변경해야 할 코드 부분은?
- 7-2
	- makeTitle() 메소드가, makeString(), makeItems(), close() 메소드가 호출되기 전에 한 번만 호출되도록 바꾸어라
	- Template Method 패턴을 이용한다
		- 상위클래스인 Builder 클래스의 makeTitle 부분에서 flag 변수인 initialized를 true로 만든다
		- Builder 클래스의 makeString(), makeItems(), close() 메소드는 initialized 변수가 true 일 때만, 하위클래스의 buildString(), buildItems(), buildResult()를 호출한다
- 7-3
	- Builder의 하위클래스를 하나 더 만들기
		- 해답: FrameBuilder
			- JFC(Java Foundation Class)을 이용해서, GUI 형태로 문서를 만듬
- 7-4
	- TextBuilder 클래스에서 String 클래스 대신에 StringBuffer 클래스를 이용한 이유는?
		- +(문자열연결) 연산자를 이용해도 된다
		- 그러나, 문자열 수정이 빈번히 일어나는 경우에는 StringBuffer를 이용하는 것이 속도가 더 빠르다  


![](https://eliotjang.github.io/assets/images/system-analysis/ch07-6.png){: width="100%"}

































