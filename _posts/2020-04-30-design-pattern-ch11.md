---
title: "[디자인 패턴] Chapter 11. Composite(복합적인) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch11. Composite 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - 컴퓨터공학
tags:
  - 디자인 패턴
last_modified_at: 2020-04-30T17:00:00+09:00  
---  
# 그릇과 내용물의 동일시

## 01. Composite 패턴

![](https://eliotjang.github.io/assets/images/system-analysis/ch08-4.png){: width="30%"}  

- 컴퓨터의 파일 시스템
  - 디렉토리(폴더) 안에 파일이나 또 다른 디렉토리가 존재한다
  - 디렉토리와 파일을 합쳐서, '디렉토리 엔트리'라고 한다

- 재귀적인 구조
  - 그릇 안에 내용물을 넣을 수도 있고, 작은 그릇을 넣을 수도 있다. 작은 그릇 안에는 더 작은 그릇이나 내용물을 넣을 수도 있다

- Composite 패턴
  - 그릇과 내용물을 동일시 해서 재귀적인 구조를 만드는 패턴
  - composite: 혼합물, 복합물이라는 뜻


## 02. 예제 프로그램

### 파일과 디렉토리를 그림으로 표현하는 프로그램

|이름|해설|
|-------------------|-----------------------------------------|
|Entry|File과 Directory를 동일시 하는 추상 클래스|
|File|파일을 나타내는 클래스|
|Directory|디렉토리를 나타내는 클래스|
|FileTreatmentException|파일에 Entry를 추가하려고 했을 때에 발생하는 예외 클래스|
|Main|동작 테스트용 클래스|

### 클래스 다이어그램

![](https://eliotjang.github.io/assets/images/system-analysis/ch11-1.png){: width="85%"}

### 기본 아이디어

![](https://eliotjang.github.io/assets/images/system-analysis/ch11-2.png){: width="20%"}

- 트리 형태의 디렉토리 구조
- 클라이언트가 root에 대해서 getSize()를 호출하면,
  - root는 자신의 내용물인 bin, tmp, usr에게 getSize()를 호출한다
  - bin은 자신의 내용물인 dir2, a.txt, b.txt에게 getSize()를 호출한다  
  ⇒ 재귀적 호출 이용

### Entry 클래스

- 추상 클래스이고, 디렉토리 엔트리를 표현함
- File 클래스와 Directory 클래스를 하위 클래스로 가진다
- 이름과 size를 가지고 있다
- add()
  - 디렉토리나 파일을 넣을 때 호출되는 메소드
  - 구현은 하위 클래스인 Directory가 제공한다
  - Entry 클래스에서는 이 메소드가 호출되면 예외를 발생시킨다
- printList(), printList(String): 오버로드 된다
  - 호출될 때 인수의 모양에 따라 적절한 메소드가 실행
  - printList(String)는 protected로 하위 클래스에서만 접근 가능
- toString(): 이름과 size를 문자열로 표현함
  - <span Style="color:blue"><b>Template Method</b> 패턴</span> (getName, getSize 추상 메소드)

```java
public abstract class Entry {
  public abstract String getName();
  public abstract int getSize();
  public Entry add(Entry entry) throws FileTreatmentException {
    throw new FileTreatmentException();
  }
  public void printList() {
    printList("");
  }
  protected abstract void printList(String prefix);
  public String toString() {
    return getName() + "(" + getsize() + ")";
  }
}
```

### File 클래스

- 파일을 표현하는 클래스
- name과 size 속성
- 생성자 File(): 이름과 크기를 인자로 받아들임
  - <span style="color:green">new File("readme.txt", 1000)</span>
- getName(): 파일의 이름을 반환
- getSize(): 파일의 크기를 반환
- PrintList(String) 구현
  - prefix와 자신의 문자열 표현을 '/'로 묶어서 표현
  - 아래의 식은 모두 동일  
  `prefix + "/" + this`  
  `prefix + "/" + this.toString()`  
  `prefix + "/" + toString()`  

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
  protected void printList(String prefix) {
    System.out.println(prefix + "/" + this);
  }
}
```

### Directory 클래스

- name 필드 존재
- size 필드는 존재하지 않는다
- getSize()에서 디렉토리 사이즈를 동적으로 계산해서 반환한다
  - 현재 디렉토리 클래스에 포함된 모든 요소(디렉토리 또는 파일)를 하나하나 꺼내서 그 사이즈를 합한다
  - 아래 코드에서, entry가 File의 인스턴스나 Directory의 인스턴스중 어떤 것이라도 상관없다
    - 이유: entry는 Entry 타입으로 선언되어 있고, Entry는 File이나 Directory의 부모 클래스이기 때문에 둘 다 참조할 수 있다  
    `size += entry.getSize();`
  - entry가 디렉토리인 경우에는, 다시 이 디렉토리의 getSize()가 재귀적으로 호출된다  
  <span style="color:red"><b>Composite</b> 패턴의 재귀적 구조와 일치함</span>  
- printList()
  - getSize 메소드와 마찬가지고 디렉토리에 포함된 Entry의 printList를 <span style="color:red"><b>재귀적으로 호출한다</b></span>
  - entry가 File의 인스턴스인지, Directory의 인스턴스인지 상관없음
    - <span style="color:blue">그릇과 내용물이 동일시 된다</span>

```java
import java.util.Iterator;
import java.util.ArrayList;

public class Directory extends Entry {
  private String name; // 디렉토리 이름
  private ArrayList directory = new ArrayList(); // 디렉토리 엔트리의 집합
  public Directory(String name) {
    this.name = name;
  }
  public String getName() { 
    // 이름을 얻는다
    return name;
  }
  public int getSize() {
    // 크기를 얻는다
    int size = 0;
    Iterator it = directory.iterator();
    while (it.hasNext()) {
      Entry entry = (Entry)it.next();
      size += entry.getSize();
    }
    return size;
  }
  public Entry add(Entry entry) {
    // 엔트리 추가
    directory.add(entry);
    return this;
  }
  protected void printList(String prefix) {
    System.out.println(prefix + "/" + this);
    Iterator it = directory.iterator();
    while (it.hasNext()) {
      Entry entry = (Entry)it.next();
      entry.printList(prefix + "/" + name);
    }
  }
}
```

### FileTreatmentException 클래스

- FileTreatmentException 클래스
- RuntimeExcepton을 상속받아서 정의됨
- Entry 클래스에 add() 메소드가 정의되어 있고, File 클래스에는 add() 메소드가 없다
  - 따라서, File 클래스에 대해서 add() 메소드가 호출되면, Entry로부터 상속받은 add()가 실행된다
  - 결국, FileTreatmentExceoption 예외가 발생

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

- 교재 203 페이지와 같은 디렉토리 계층을 만든다. 그 후에, 이 디렉토리 구조를 출력  
`rootdir.printList();`  
  - 재귀적으로, 디렉토리 안에 있는 요소(디렉토리나 파일)의 printList()가 호출된다

```java
public class Main {
  public static void main(String[] args) {
    try{
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
      rootdir.printList();

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
      Park.add(new File("game.doc", 400));
      Park.add(new File("junk.mail", 500));
      rootdir.printList();
    } catch (FileTreatmentException e) {
      e.printStackTrace();
    }
  }
}
```

![](https://eliotjang.github.io/assets/images/system-analysis/ch11-3.png){: width="80%"}

## 03. 등장 역할

- Leaf(잎사귀) 역할
  - '내용물'에 해당되는 역할
  - 이 안에는 다른 것을 넣을 수 없다
  - 예제에서는 File 클래스가 해당됨

- Composite(복합체) 역할
  - '그릇'을 나타내는 역할
  - Leaf와 Composite을 넣을 수 있다
  - 예제에서는 Directory 클래스가 해당됨

- Component의 역할
  - Leaf 역할과 Composite 역할을 동일시 하기 위한 역할
  - Leaf와 Composite의 상위 클래스로 구현됨
  - 예제에서는 Entry 클래스가 해당됨

- Client(의뢰자)의 역할
  - 예제에서는 Main 클래스가 해당됨

![](https://eliotjang.github.io/assets/images/system-analysis/ch11-4.png){: width="100%"}

## 04. 독자의 사고를 넓혀주는 힌트

### 복수와 단수의 동일시

![](https://eliotjang.github.io/assets/images/system-analysis/ch08-4.png){: width="30%"}

- Composite 패턴은, 그릇과 내용물을 동일시 하는 패턴
  - 그릇이, 또 다른 그릇의 내용물이 될 수 있다

### add를 어디에 두어야 하나?

- 방법1: Entry 클래스에서 구현하고, 에러로 처리한다
  - 예제 프로그램이 여기에 해당
- 방법2: Entry 클래스에서 구현하고, 아무것도 실행하지 않는다
- 방법3: Entry 클래스에서 추상 메소드로 선언은 하지만, 구현은 하지 않는다
  - 하위 클래스에서 필요하면 정의하고, 필요하지 않으면 에러로 처리한다
- 방법4: Directory 클래스에만 넣는다
  - 이 방법은, Entry형의 변수에 add할 때, Directory형으로 일일이 타입캐스트(형변환)해야 하는 번거로움이 있다  
  `(Directory)entry.add();`  

### 재귀적 구조는 여러 곳에서 등장한다

- 예: 윈도우 안에는 자식 윈도우를 가진다
- 일반적으로 트리 구조는 Composite 패턴에 속한다

## 05. 관련 패턴
- Command 패턴(22장)
- Visitor 패턴(13장)
- Decorator 패턴(12장)

## 06. 요약
- 그릇과 내용물을 동일시하고, 재귀적인 구조를 형성하는 Composite 패턴

## 연습문제
- 각자 공부할 것































