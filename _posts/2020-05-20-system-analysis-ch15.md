---
title: "[시스템분석및설계] Chapter 15. Facade 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch15. Facade 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-05-20T15:00:00+09:00  
---  

## 01. Facade 패턴

- 객체 지향 프로그램은 많은 클래스와 객체들로 이루어지고, 서로 복잡한 관련을 맺고 있다
  - 이들을 모두 이해하고 제어하기가 힘들다
  - 이들을 제어하기 위한 '창구' 역할을 담당하는 클래스를 만들어보자

- facade: 건물의 정면(불어에서 따옴)

- 복잡한 내부는 숨기고, 높은 레벨의 인터페이스(API)를 외부에 제공한다

![](https://eliotjang.github.io/assets/images/system-analysis/ch15-1.png){: width="100%" }


## 02. 예제 프로그램

- 사용자의 웹페이지를 작성하는 프로그램
- Facade 패턴의 예를 보이기 위해서는 "복잡하게 얽혀 있는 많은 클래스"가 필요하다
  - 그러나, 본 예제에서는 3개의 클래스로 구성된 시스템을 생각한다

|패키지|이름|해설|
|------|----|----|
|pagemaker|Database|메일 주소로부터 사용자명을 얻는 클래스|
|pagemaker|HtmlWriter|HTML 파일을 작성하는 클래스|
|pagemaker|PageMaker|메일 주소로부터 사용자의 웹 페이지를 작성하는 클래스|
|Anonymous|Main|동작 테스트용 클래스|

> <span style="color:red">PageMaker가 Facade 역할(High-level API 제공)</span>

- 만들고자 하는 웹페이지

![](https://eliotjang.github.io/assets/images/system-analysis/ch15-2.png){: width="80%" }

- 클래스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch15-3.png){: width="100%" }

- 소스파일 디렉토리 구조

![](https://eliotjang.github.io/assets/images/system-analysis/ch15-4.png){: width="80%" }

### Database 클래스
  
- 데이터베이스 명을 지정하면, 그것에 해당하는 Properties를 작성하는 클래스
- getProperties(String dbname)
  - Properties 인스턴스를 생성한 후, dbname.txt 파일로부터 여러 가지 속성 값을 읽어 들여 이를 리턴하는 메소드
  - Properties 클래스
    - key와 value 쌍으로 되어 있는 속성의 집합을 유지하는 클래스

```java
package pagemaker;

import java.io.FileInputStream;
import java.io.IOException;
import java.util.Properties;

public class Database {
  private Database() {
  }
  public static Properties getProperties(String dbname) {
    String filename = dbname + ".txt";
    Properties prop = new Properties();
    try {
      prop.load(new FileInputStream(filename));
    } catch (IOException e) {
      System.out.println("Warning: " + filename + "is not found.");
    }
    return prop;
  }
}
```

### maildata.txt

- 속성을 저장하고 있는 파일
- 형식: `key=value`

![](https://eliotjang.github.io/assets/images/system-analysis/ch15-5.png){: width="60%" }

### HtmlWriter 클래스

- 간단한 웹페이지를 작성하는 클래스
- 생성자
  - Writer 타입의 인스턴스를 받아들여, 멤버 변수인 writer에 할당한다
  - title(): 헤더 및 타이틀에 대한 html 태그를 작성하는 메소드
  - paragraph(): 문단을 작성하는 메소드
  - link(): 하이퍼링크를 만드는 메소드
  - mailto(): 메일주소 링크를 만드는 메소드
  - close(): HTML 출력을 끝내는 메소드
  - 제약조건: title() 메소드가 제일 먼저 호출되어야 한다
    - 이 조건은, 창구가 되는 PageMaker 클래스에 표현되어 있다

```java
package pagemaker;

import java.io.Writer;
import java.io.IOException;

public class HtmlWriter {
  private Writer writer;
  public HtmlWriter(Writer writer) {
    this.writer = writer;
  }
  public void title(String title) throws IOException {
    writer.write("<html>");
    writer.write("<head>");
    writer.write("<title>" + title  + "</title>");
    writer.write("<body>\n");
    writer.write("<h1>" + title + "</h1>\n");
  }
  public void paragraph(String msg) throws IOException {
    writer.write("<p>" + msg + "</p>\n");
  }
  public void link(String href, String caption) throws IOException {
    paragraph("<a href=\"" + href + "\">" + caption + "</a>");
  }
  public void mailto(String mailaddr, String username) throws IOException {
    link("mailto:" + mailaddr, username);
  }
  public void close() throws IOException {
    writer.write("</body>");
    writer.write("</html>\n");
    writer.close();
  }
}
```

### PageMaker 클래스

- Database 클래스와 HtmlWriter 클래스를 조합하여, 지정된 사용자의 웹 페이지를 작성하기 위한 클래스
- makeWelcomePage(String mailaddr, String filename)
  - Database 클래스를 이용해서
    - "maildata.txt" 파일로부터 속성 집합을 얻은 후, 속성 중에서 입력 인자로 들어온 mailaddr를 key로 하여 해당 value를 얻는다
  - HtmlWriter 클래스의 메소드들을 이용해서
    - 입력 인자로 들어온 filename 파일에 HTML 문서를 작성한다

```java
package pagemaker;

import java.io.FileWriter;
import java.io.IOException;
import java.util.Properties;

public class PageMaker {
  private PageMaker() {
  }
  public static void makeWelcomePage(String mailaddr, String filename) {
    try {
      Properties mailprop = Database.getProperties("maildata");
      String username = mailprop.getProperty(mailaddr);
      HtmlWriter writer = new HtmlWriter(new FileWriter(filename));
      writer.title("Welcome to " + username + "'s page!");
      writer.paragraph("메일을 기다리고 있습니다.");
      writer.mailto(mailaddr, username);
      writer.close();
      System.out.println(filename + " is created for " + mailaddr + " (" + username + ")");
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

### Main 클래스

- pagemaker 패키지의 PageMaker 클래스를 이용해서, eliotjang2@gmail.com를 key로 한 value를 가지고 "welcome.html" 이라는 HTML 문서를 완성한다
- Main 클래스는, Database나 HtmlWriter 클래스를 직접 이용하지 않고, high-level API를 제공하는 PageMaker의 메소드만을 사용하여 원하는 작업을 한다

```java
import pagemaker.PageMaker;

public class Main {
  public static void main(String[] args) {
    PageMaker.makeWelcomePage("eliotjang2@gmail.com", "welcome.html");
  }
}
```

페이지화면과 명령창 출력 사진 요망

## 03. Facade(정면)의 역할

- 시스템을 구성하는 많은 역할의 '간단한 창구'가 되는 역할을 한다
- 높은 레벨에서 간단한 인터페이스를 시스템 외부에 제공한다
- 예제에서는 PageMaker 클래스가 해당됨

- 시스템을 구성하고 있는 그 밖의 많은 역할
  - Facade 역할로부터 호출되는, 시스템의 구성하는 많은 클래스들
  - 이들은, Facade 역할은 의식하지 않는다
  - 예제에서는, Database와 HtmlWriter 클래스가 해당됨

- Client(의뢰자)의 역할
  - Facade 패턴을 이용하는 역할
  - 예제에서는, Main 클래스가 해당됨

- 클래스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch15-6.png){: width="100%" }

## 04. 독자의 사고를 넓혀주는 힌트

- Facade 역할이 하는 일은 무엇일까
  - 복잡한 것을 단순하게 보여준다
    - 내부에서 작동하고 있는 많은 클래스들의 관계나 사용법을 의식하지 않도록 해 주는 역할
    - 외부에 보이는 API를 적게 해 주고 단순하게 해 준다

- 재귀적인 Facade 패턴의 적용
  - 여러 패키지들이 있고, 패키지마다 Facade 역할이 정의되어 있다면, 이들 Facade 들에 대한 Facade 역할을 재귀적으로 정의할 수 있다

## 05. 관련 패턴

- Abstract Factory 패턴 (8장)
- Singleton 패턴 (5장)
- Mediator 패턴 (16장)

## 06. 요약

- 복작한 시스템에 대한 간단한 창구의 역할을 하는 Facade 패턴

## 연습 문제

- 15-1
  - 외부에서 절대로 Database 클래스나 HtmlWriter 클래스를 이용할 수 없도록 하려면, 어떻게 예제 프로그램을 변경해야 하는가?

- 15-2
  - maildata.txt의 내용을 이용해서, 메일 주소 링크 만을 포함하는 파일을 만드는 makeLinkPage() 메소드를 PageMaker 클래스에 추가하기
    - 예: 그림 15-7, 15-8, 15-9


