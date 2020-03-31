---
title: "[시스템분석및설계] Chapter 1. 이터레이터(Iterator) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-03-30T19:00:00+09:00  
---  

## 01. Iterator 패턴  

**for 문의 변수 i의 역할**  

```java
for (int i = 0; i < arr.length; i++) {
  System.out.println(arr[i]);
}
```  
  - 변수 i: 여러 원소가 모여있는 배열의 각 원소를 차례대로 선택하는 사용된다.  

**Iterator 패턴**
  - 변수 i의 기능을 추상화해서 일반화 시켰음
  - 무엇인가 많이 모여있는 것 중에서 하나씩 끄집어내서 열거하면서 전체를 처리하는 일을 할 때 이 패턴을 적용한다.  

## 02. 예제 프로그램  

**책꽂이(BookShelf)에 책(Book)을 넣은 후, 순서대로 다시 끄집어 내서 책 이름을 표시하는 프로그램**  

![](https://eliotjang.github.io/assets/images/system-analysis/ch01-1.png){: width="70%" height="50%"}  

**각 클래스와 인터페이스 설명**  

|이름|해설|
|----|----|
|Aggregate|집합체를 나타내는 인터페이스|
|Iterator|하나씩 열겨하면서 스캔을 실행하는 인터페이스|
|Book|책을 나타내는 클래스|
|BookShelf|서가를 나타내는 클래스|
|BookShelfIterator|서가를 검색하는 클래스|
|Main|동작 테스트용 클래스|  

![](https://eliotjang.github.io/assets/images/system-analysis/ch01-2.png){: width="70%" height="70%"}   

> 각 인터페이스나 클래스를 <font color="blue">클릭</font>하여 코드와 기능을 상세하게 볼 수 있습니다.  
<details>
<summary><font color="blue">Aggregate 인터페이스</font></summary>
<div markdown="1">

**Aggregate 인터페이스 (sample/Aggregate.java)**  
  - iterator(): 집합체에 대응하는 Iterator 한 개를 생성하는데 사용될 메소드
      - 어떤 집합체 원소를 하나씩 열거하거나 조사하고자 할 때 이 메소드를 사용해서 Iterator 인터페이스를 구현한 클래스의 인스턴스를 한 개 얻어온다.  

```java
public interface Aggregate {
  public abstract Iterator iterator();
}
```  

</div>
</details>  

<details>
<summary><font color="blue">Iterator 인터페이스</font></summary>
<div markdown="1">

**Iterator 인터페이스 (sample/Iterator.java)**  
  - 집합체의 원소를 하나하나 끄집어내는 루프 변수와 같은 역할을 한다.
  - hasNext(): 다음 원소가 존재하는지 조사할 때 사용하는 메소드
      - 반환형은 boolean (마지막 원소에 도달하면 false를 반환함)
  - next(): 다음 원소를 얻어올 때 사용하는 메소드
      - 반환형은 Object  

```java
public interface Iterator {
  public abastract boolean hasNext();
  publci abstract Object next();
}
```
</div>
</details>  

<details>
<summary><font color="blue">Book 클래스</font></summary>
<div markdown="1">

**Book 클래스(sample/Book.java)**  
  - 책을 나타내는 클래스
  - name 속성: 책이름을 저장하는 변수
  - getName(): 책의 이름을 얻어올 때 호출하는 메소드  

```java
public class Book {
  private String name;
  public Book(String name) {
    this.name = name;
  }
  public String getName() {
    return name;
  }
}
```  

</div>
</details>

<details>
<summary><font color="blue">BookShelf 클래스</font></summary>
<div markdown="1">

**BookShelf 클래스 (sample/BookShelf.java)**  
  - 책꽂이를 나타내는 클래스 = 집합체(aggregate)
  - Aggregate 인터페이스를 구현하였다.
      - BookShelf 클래스는, iterator() 메소드의 구현 부분을 제공한다.
      - BookShelf 클래스는, 이 외에 다른 추가의 메소드도 제공한다.
  - books 필드: Book의 배열
      - 이 배열의 크기는, 생성자(BookShelf()) 호출 시 지정된다.
      - private으로 선언된 이유는, 클래스 외부에서 이 필드를 변경하지 못하게 하기 위해서이다.
  - appendBook(): 책 한권을 서가에 추가하는 메소드
  - getLength(): 현재 책꽂이에 있는 책의 개수를 반환하는 메소드
  - iterator(): 책꽂이의 책 하나하나를 끄집어내는 일을 하는 BookShelfIterator를 생성하는 메소드  

```java
public class BookShelf implements Aggregate {
  private Book[] books;
  private int last = 0;
  public BookShelf(int maxsize) {
    this.books  = new Book[maxsize];
  }
  public Book getBookAt(int index) {
    return books[index];
  }
  public void appendBook(Book book) {
    this.books[last] = book;
    last++;
  }
  public int getLength() {
    return last;
  }
  public Iterator iterator() {
    return new BookShelfIterator(this);
  }
}
```  

</div>
</details>

<details>
<summary><font color="blue">BookShelfIterator 클래스</font></summary>
<div markdown="1">

**BookShelfIterator 클래스 (sample/BookShelfIterator.java)**  
  - 책꽂이(BookShelf)에 있는 책들을 하나씩 끄집어내는 일을 하는 클래스
  - Iterator 인터페이스를 구현하였다.
      - hasNext()와 next() 메소드를 구현함.
  - BookShelf 필드: BookShelfIterator가 검색할 책꽂이를 가리키는 변수(생성자에서 넘겨받은 BookShelf의 인스턴스를 가지고 있음)
  - index 필드: 책꽂이에서의 현재 책을 가리키는 변수
  - hasNext(): 다음 책이 있으면 true, 없으면 false를 반환함
  - next(): 현재 가리키고 있는 책을 반환하고, 다음 책을 가리키는 메소드  

```java
public class BookShelfIterator implements Iterator {
  private BookeShelf bookShelf;
  private int index;
  public BookShelfIterator(BookShelf bookShelf) {
    this.bookShelf = bookShelf;
    this.index = 0;
  }
  public boolean hasNext() {
    if (index < bookShelf.getLength()) {
      return true;
    } else {
      return false;
    }
  }
  public Object next() {
    Book book = bookShelf.getBookAt(index);
    index++;
    return book;
  }
}
```  

</div>
</details>

<details>
<summary><font color="blue">Main 클래스</font></summary>
<div markdown="1">

**Main 클래스 (sample/Main.java)**  
  - main()
      1. 책 4권을 책꽂이 넣는다.
      2. 책꽂이의 책을 하나씩 끄집어낼 Iterator를 얻는다.
	  - `Iterator it = bookShelf.iterator();`
      3. Iterator의 hasNext()와 next() 메소드를 이용하여 책을 하나씩 끄집어내서 책의 이름을 출력한다.  

```java
import java.util.*;

public class Main {
  public static void main(String[] args) {
    BookShelf bookShelf = new BookShelf(4);
    bookShelf.appendBook(new Book("Around the World in 80 Days"));
    bookShelf.appendBook(new Book("Bible"));
    bookShelf.appendBook(new Book("Cinderella"));
    bookShelf.appendBook(new Book("Daddy-Long-Legs"));
    Iterator it = bookShelf.iterator();
    while (it.hasNext()) {
      Book book = (Book)it.next();
      System.out.println(book.getName());
    }
  }
}
```  

</div>
</details>

## 03. Iterator 패턴에 등장하는 역할  

**Iterator(반복자)의 역할**  
  - 원소를 하나씩 끄집어낼 때 사용할 공통된 메소드를 선언한 인터페이스
  - hasNext()와 next() 메소드 선언  

**ConcreteIterator(구체적인 반복자)의 역할**  
  - Iterator 인터페이스를 실제로 구현하는 클래스
  - BookShelfIterator가 이 역할을 담당하였다.  

**Aggregate(집합체)의 역할**  
  - Iterator를 만들어내는 인터페이스를 제공함.
  - iterator(): 내가 가지고 있는 각 원소들을 차례로 검색해 줄 사람을 만들어내는 메소드  

**ConcreteAggregate(구체적인 집합체)의 역할**  
  - Aggregate 인터페이스를 구현하는 클래스
  - ConcreteIterator(구체적인 반복자) 객체를 생성한다.
  - BookShelf가 이 일을 담당하였다.

![](https:///Users/eliotjang/Projects/eliotjang.github.io/assets/images/system-analysis/ch01-3.png){: width="60%" height="40%"}  


## 04. 독자의 사고를 넓혀주는 힌트  

**왜 Iterator 패턴이 유용한가?**  
  - 집합체가 원소를 어떻게 구현하고 있든지 상관없이, 집합체의 원소를 차례로 끄집어내고자 하면, Iterator의 hasNext()와 next() 메소드를 사용하면 된다.
      - <font color="blue">while루프는 BookShelf 구현과 무관</font>
  - 예: BookShelf가 Book을 배열에 저장하지 않고, vector에 저장하도록 수정하더라도, Main 클래스의 main() 메소드의 다음 부분을 변경하지 않아도 된다.
      - <font color="blue">while루프는 변경하지 않음</font>
      ```java
      while (it.hasNext() ) {
	Book book = (Book)it.next();
	System.out.println("" + book.getName());
      }
      ```  
  - 집합체의 각 원소를 끄집어내는 방법이 집합체 구현과 무관하다.
  - 하나를 수정했을 때, 수정해야 할 부분을 저게 하는 것이 중요하다.  



**<font color="red">코드의 어떤 부분을 수정했을 때, 그것으로 인해 수정해야 할 코드 부분을 적게 하는 것이 중요하다.</font>**  


















