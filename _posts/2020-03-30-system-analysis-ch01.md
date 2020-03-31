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

**Aggregate 인터페이스 (sample/Aggregate.java)**  

  - iterator(): 집합체에 대응하는 Iterator 한 개를 생성하는데 사용될 메소드
      - 어떤 집합체 원소를 하나씩 열거하거나 조사하고자 할 때 이 메소드를 사용해서 Iterator 인터페이스를 구현한 클래스의 인스턴스를 한 개 얻어온다.  

```java
public interface Aggregate {
  public abstract Iterator iterator();
}
```  

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

















