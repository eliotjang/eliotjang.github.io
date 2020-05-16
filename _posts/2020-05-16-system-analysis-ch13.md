---
title: "[시스템분석및설계] Chapter 13. Visitor 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch13. Visitor 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-05-16T13:00:00+09:00  
---  

## 01. Visitor 패턴

- 데이터 구조 안에 저장되어 있는 많은 요소에 대해서, 무언가 "처리"해 나가고자 한다
    - 얼핏 생각하면, 데이터 구조를 나타내고 있는 클래스 안에 "처리"를 기술해야 한다고 생각할 것이다
    - 그러나, "처리"가 여러 종류라고 한다면?
	- 새로운 처리가 필요해질 때마다, 데이터 구조를 나타내는 클래스를 수정해야 한다

- 데이터 구조와 처리를 분리하자
    - 데이터 구조를 돌아다니면 "방문자"를 정의해서, 이 방문자가 "처리"를 담당하도록 하자
    - 새로운 처리를 추가하고 싶을 때는, 새로운 "방문자"를 만든다

- 데이터 구조는, 문을 두드리는 "방문자"를 받아들이기만 하면 된다


## 02. 예제 프로그램

- 방문자가 방문하는 데이터 구조
    - 11장 Composite 패턴의 예제에서 사용된 파일과 디렉토리를 다시 사용한다

- 파일과 디렉토리로 구성된 데이터 구조를 방문자가 방문하면서, 파일 리스트를 출력한다

|이름|해설|
|---|----|
|Visitor|파일이나 디렉토리를 방문하는 방문자를 나타낸 추상 클래스|
|Element|Visitor 클래스의 인스턴스를 받아들이는 데이터 구조를 나타내는 인터페이스|
|ListVisitor|Visitor 클래스의 하위 클래스로 파일이나 디렉토리의 종류를 나타내는 클래스|
|Enrty|File과 Directory의 상위 클래스가 되는 추상 클래스(Element 인터페이스를 구현)|
|File|파일을 나타내는 클래스|
|Directory|디렉토리를 나타내는 클래스|
|FileTreatmentException|File에 대해서 add한 경우에 발생하는 예외 클래스|
|Main|동작 테스트용 클래스|

![](https://eliotjang.github.io/assets/images/system-analysis/ch13-1.png){: width="100%" }  

### Visitor 클래스

- 방문자를 나타내는 추상 클래스
- visit(File)
    - File을 방문했을 때 File 클래스가 호출하는 메소드
- visit(Directory)
    - Directory를 방문했을 때 Directory 클래스가 호출하는 메소드
- 메소드 오버로드(overload) 이용했음

```java
public abstract class Visitor {
    public abstract void visit(File file);
    public abstract void visit(Directory directory);
}
```

### Element 인터페이스

- 방문자를 받아들이는 클래스를 위한 인터페이스
- accept(Visitor v)
    - Visitor 타입을 입력 인자로 받아들임

```java
public interface Element {
    public abstract void accept(Visitor v);
}
```

### Entry 클래스

- Composite 패턴에 등장했던 File이나 Directory를 위한 추상 클래스
- Element 인터페이스를 구현함
- add()
    - Directory 클래스에서만 add()가 유효하므로, Entry 클래스에서는 에러로 처리한다
- iterator()
    - 요소를 얻을 때 사용하는 메소드
    - Directory 클래스에서만 iterator()가 유효하므로, Entry 클래스에서는 에러로 처리한다

```java
import java.util.Iterator;

public abstract class Entry implements Element {
    public abstract String getName();
    public abstract int getSize();
    public Entry add(Entry entry) throws FileTreatmentException {
	throw new FileTreatmentException();
    }
    public Iterator iterator() throws FileTreatmentException {
	throw new FileTreatmentException();
    }
    public String toString() {
	return getName() + "(" + getSize() + ")";
    }
}
```

### File 클래스

- accept(Visitor)
    - 클라이언트가, File 객체에게 "방문자를 받아들이세요"라고 요청할 때 호출하는 메소드
	- 클라이언트는 Visitor를 매개변수로 하여 accept를 호출할 것이다
    - 입력 인자로 들어온 방문자의 visit 메소드를 호출한다
	- 이 때, 현재 자신 객체를 인자로 하여 호출한다
	- 그러면, Visitor의 visit(File) 메소드가 실행된다
	- 즉, 방문자가 방문하면 방문자에게 "나는 File 객체입니다. 나의 일을 처리해 주세요"라고 요쳥하는 것과 비슷하다

```java
public class File extends Entry {
    private String name;
    private int size;
    public File(String name, int size) {
	this.name = name;
	this.size = size;
    }
    public String getName() {
	return name;
    }
    public int getSize() {
	return size;
    }
    public void accept(Visitor v) {
	v.visit(this);
    }
}
```

### Directory 클래스

- iterator()
    - 디렉토리가 유지하고 있는 엔트리들에 대한 Iterator를 반환한다

- accept(Visitor)
    - 입력 인자로 들어온 Visitor에게, 자기 자신을 매개 변수로 해서 visit()를 호출한다
    - 그러면, Visitor의 visit(Directory) 메소드가 실행된다
	- 방문자가 방문하면 방문자에게 "나는 Directory 객체입니다. 나의 일을 처리해 주세요"라고 요청하는 것과 비슷하다

```java
import java.util.Iterator;
import java.util.ArrayList;

public class Directory extends Entry {
    private String name;
    private ArrayList dir = new ArrayList();
    public Directory(String name) {
	this.name = name;
    }
    public String getName() {
	return name;
    }
    public int getSize() {
	int size = 0;
	Iterator it = dir.iterator();
	while (it.hasNext()) {
	    Entry entry = (Entry)it.next();
	    size += entry.getSize();
	}
	return size;
    }
    public Entry add(Entry entry) {
	dir.add(entry);
	return this;
    }
    public Iterator iterator() {
	return dir.iterator();
    }
    public void accept(Visitor v) { // 방문자 승낙
	v.visit(this);
    }
}
```

### ListVisitor 클래스

- Visitor 클래스의 하위 클래스
- 실제 데이터 구조(File이나 Directory)를 옮겨 다니면서, 리스트를 출력하는 일을 한다
- currentdir 필드
    - 현재 주목하고 있는 디렉토리 명을 저장함
- visit(File)
    - 입력인자로 받아들인 "File 에 대해 수행해야 할 처리"가 기술되어 있다
    - 알고리즘
	- 현재 디렉토리와 File의 toString() 반환 값을 연결하여
	- 현재 파일의 전체 경로를 출력한다
- visit(Directory)
    - 입력 인자로 받아들인 "Directory에 대해 수행해야 할 처리"가 기술되어 있다
    - 알고리즘
	- 먼저, 디렉토리 전체 경로를 출력한다
	- 현재 디렉토리(currentdir)를 임시로 savedir에 저장한다
	- 현재 디렉토리(currentdir)를, 입력 인자로 들어온 디렉토리로 바꾼다
	- 입력 인자로 들어온 디렉토리의 iterator를 얻는다
	- 입력 인자로 들어온 디렉토리가 유지하는 원소드를 차례로 방문하면서, accept(this)를 호출하여 방문자가 방문했음을 알린다
	- while 루프가 끝나면, currentdir을 원래 디렉토리로 복귀시킨다
    - 복잡한 재귀적인 호출
	- accpet 메소드는 visit 메소드를 호출하고, visit 메소드는 accept 메소드를 호출한다

```java
import java.util.Iterator;

public class ListVisitor extends Visitor {
    private String currentdir = "";
    public void visit(File file) {
	System.out.println(currentdir + "/" + file);
    }
    public void visit(Directory directory) {
	System.out.println(currentdir + "/" + directory);
	String savedir = currentdir;
	currentdir = currentdir + "/" + directory.getName();
	Iterator it = directory.iterator();
	while (it.hasNext()) {
	    Entry entry = (Entry)it.next();
	    entry.accept(this);
	}
	currentdir = savedir;
    }
}
```

### FileTreatmentException 클래스

- File 엔트리에 무엇인가 추가(add)하고자 할 때 발생되는 예외

```java
public class FileTreatmentException extends RuntimeException {
    public FileTreatmentException() {
    }
    public FileTreatmentException(String msg) {
	super(msg);
    }
}
```

### Main 클래스

- Composite 패턴에서는,
    - main() 메소드가 엔트리 리스트를 출력하기 위해서 rootdir.printList()를 호출하였다
    - 엔트리의 내용을 출력하는 책임을, File이나 Directory 클래스가 가지고 있다
- Visitor 패턴에서는,
    - main() 메소드가 엔트리 리스트를 출력하기 위해서, rootdir.accept(new ListVisitor())를 호출하였다
    - 엔트리의 내용을 출력하는 책임을, ListVisitor 클래스가 가지고 있다

```java
public class Main {
    public static void main(String[] args) {
	try {
	    System.out.println("Making root entries...");
	    Directory rootdir = new Directory("root");
	    Directory bindir = new Directory("bin");
	    Directory tmpdir = new Directory("tmp");
	    Directory usrdir = new Directory("usr");
	    rootdir.add(bindir);
	    rootdir.add(tmpdir);
	    rootdir.add(usrdir);
	    bindir.add(new File("vi", 10000));
	    bindir.add(new File("latex", 20000));
	    rootdir.accept(new ListVisitor());

	    System.out.println("");
	    System.out.println("Making user entries...");
	    Directory Kim = new Directory("Kim");
	    Directory Lee = new Directory("Lee");
	    Directory Park = new Directory("Park");
	    usrdir.add(Kim);
	    usrdir.add(Lee);
	    usrdir.add(Park);
	    Kim.add(new File("diary.html", 100));
	    Kim.add(new File("Composite.java", 200));
	    Lee.add(new File("memo.tex", 300));
	    Park.add(new File("gmae.doc", 400));
	    Park.add(new File("junk.mail", 500));
	    rootdir.accept(new ListVisitor());
	} catch (FileTreatmentException e) {
	    e.printStackTrace();
	}
    }
}
```

![](https://eliotjang.github.io/assets/images/system-analysis/ch13-2.png){: width="80%" }

### Visitor와 Element의 상호 호출

- 하나의 Directory와 두 개의 File이 있는 경우의 시퀀스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch13-3.png){: width="100%" }


## 03. 등장 역할

- Visitor(방문자)의 역할
    - 데이터 구조 내의 각각의 구체적인 요소(ConcreteElement 역할)에 visit(xxx) 메소드를 선언하는 역할
    - visit(xxx) 메소드는, 실제 xxx를 처리하기 위한 실제 코드가 하위 클래스에 의해 제공된다
    - 예제에서는 Visitor 클래스가 해당됨

- ConcreteVisitor(구체적 방문자)의 역할
    - Visitor 역할의 인터페이스(API)를 실제로 구현하는 역할
    - 예제에서는 ListVisitor 클래스가 해당됨

- Element(요소)의 역할
    - Visitor 역할이 방문할 장소를 나타내는 역할
    - 방문자를 받아들이는 accept(Visitor) 메소드를 선언한다
    - 예제에서는, Element 인터페이스가 해당됨

- ConcreteElement(구체적인 요소)의 역할
    - Element 역할의 인터페이스를 구현하는 역할
    - 예제에서는, File이나 Directory 클래스가 해당됨

- ObjectStructure(객체의 구조)의 역할
    - Element 역할을 집합으로 취급할 수 있도록 해 주는 메소드를 제공한다
    - 예제에서는, Directory 클래스가 해당됨
	- 유지하고 있는 요소들을 얻어갈 수 있도록, iterator 메소드를 제공함

- 클래스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch13-4.png){: width="100%" }

- 더블 디스패치(dispatch)
    - dispatch: 급파하다, 발송하다, 신속히 처리하다
    - Visitor의 Element는 서로 대응 관계
	- ConcreteElement와 ConcreteVisitor의 역할을 하는 한 쌍에 의해 실제의 처리가 결정된다

![](https://eliotjang.github.io/assets/images/system-analysis/ch13-5.png){: width="80%" }


## 04. 독자의 사고를 넓혀주는 힌트

- 왜 이렇게 복잡한 일을 하는가?
    - "반복 처리가 필요하면 데이터 구조 안에 루프를 쓰면 되지 않나?"
    - Visitor 패턴의 목적은, "처리"를 "데이터 구조"로부터 분리하는 것이다
	- 다른 처리를 하는 ConcreteVisitor를 추가할 수 있다
	- 또, 기존의 ConcreteVisitor의 기능을 확장하기도 쉽다
	- 결국, 부품의 독립성을 높여준다

- Open-Closed Principle
    - 확장에 대해서는 열려있고
	- 클래스를 설계할 때에 특별한 이유가 없는 한 장래의 확장을 허락해야 한다
    - 수정에 대해서는 닫혀있다
	- 확장을 하더라도 기존의 클래스는 수정할 필요가 없어야 한다

- ConcreteVisitor 역할의 추가는 간단하지만, ConcreteElement 역할의 추가는 곤란하다


## 05. 관련 패턴

- Iterator 패턴
- Composite 패턴
- Interpreter 패턴(23장)


## 06. 요약

- 데이터 구조 안을 돌아다니면서 수행하는 Visitor 패턴


## 연습 문제

- 13-1 : FileFindVisitor 클래스 추가하기
    - 지정된 확장자의 파일을 모으는 방문자

- 13-2 : SizeVisitor 클래스 도입하기
    - "사이즈를 얻는 처리"를 하는 방문자
    - Directory의 getSize 메소드를 응용할 것

## Homework : Visitor 패턴 응용

- FileNameFindVisitor 클래스 추가하기
  - 파일 이름에 "생성자의 입력 인자 name"이 포함된 모든 파일을 모으는 일을 하는 방문자
  - 구현 방법
    - 연습문제 13-1 응용하기
    - 생성자 FileNameFindVisitor(String name)
  - FileNameFindVisitor.getFoundFiles() 메소드
    - FileNamedFindVisitor가 모은 모든 파일에 대한 Iterator를 얻어올 때 호출하는 메소드
  - Main 클래스
    - Main.main()은 리스트 13-9와 같은 형태
    - 그림 13-2와 같은 디렉토리 구조에, "Kim", "Lee", "Park" 디렉토리 안에 각각 "hyejaKim.txt", "Leehyeja.txt", "Parkhyeja.tat" 파일을 추가한다
    - 그리고 나서, 파일 이름이 "hyeja"를 포함하는 파일을 모두 출력한다

![](https://eliotjang.github.io/assets/images/system-analysis/ch13-6.png){: width="80%" }

- 숙제 제출 방법

  - __보고서__
  - 표지 (과목명, 학번, 이름, 제출일, 담당교수 등)
  - 코드 설명 (표를 이용하는 등 보기 좋게 작성)
    - 각 클래스가 하는 일
    - 각 클래스의 각 메소드가 하는 일
  - 코드 프린트

  - __프로그램__
  - 해람인의 e참뜰 과제물 제출 기능을 이용
    - 바이러스 체크 후 압축파일로 제출
    - 각 클래스의 소스 파일 (java)
    - 모든 클래스 컴파일 한 파일 (class)
