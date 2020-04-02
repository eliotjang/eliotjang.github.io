---
title: "[시스템분석및설계] Chapter 4.  팩토리(Factory) 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

categories: 
  - system analysis
tags:
  - system analysis
last_modified_at: 2020-04-01T18:00:00+09:00  
---  

## 01. Factory Method 패턴  

**Factory Method 패턴**
  - Template Method를 변형한 패턴
  - 인스턴스 만드는 방법은 상위 클래스에서 결정하고
  - 인스턴스를 실제로 생성하는 일은 하위 클래스에서 결정한다.
  - '구체적인 제품 생성'을 '공장'을 통해서 한다.  


## 02. 예제 프로그램  

**신분증(ID 카드)를 만드는 공장**
  - framework 패키지
      - Product / Factory
  - idcard 패키지
      - IDCard / IDCardFactory
  - IDCard 객체를 Main 클래스에서 직접 생산할 수 있다.
  - 그러나, 우리는 Factory Method 패턴을 이용해서,
      - IDCard 객체가 필요하면, IDCardFactory를 통해서 IDCard 제품을 생산하도록 하겠다.  

|패키지|이름|해설|
|---------|-------------|-------------------------------------------|
|framework|Product|추상 메소드 use만 정의되어 있는 추상 클래스|
|framework|Factory|메소드 create를 구현하고 있는 추상 클래스|
|idcard|IDCard|메소드 use를 구현하고 있는 클래스|
|idcard|IDCardFactory|메소드 createProduct, registerProduct를 구현하고 있는 클래스|
|Anonymous|Main|동작 테스트용 클래스|  

**클래스 다이어그램**  

![](https://eliotjang.github.io/assets/images/system-analysis/ch04-1.png){: width="614" height="328"}  

> 해당 클래스를 **<font color="blue">클릭</font>**하여 기능과 코드를 살펴볼 수 있습니다.  

<details>
**<summary><font color="blue">Product 클래스</font></summary>**
<div markdown="1">

**Product 클래스**
  - <font color="blue">framework 패키지</font>
  - '제품'을 표현한 추상 클래스
  - use()의 구현은 하위 클래스에 맡겨짐  

```java
package framework;

public abstract class Product {
  public abstract void use();
}
```  

</div>
</details>

<details>
**<summary><font color="blue">Factory 클래스</font></summary>**
<div markdown="1">

**Factory 클래스**
  - <font color="blue">framework</font> 패키지
  - create()
      - <font color="red">Template Method 패턴</font> 사용됨
	  - 추상 메소드인 createProduct와 registerProduct를 사용함
      - 제품을 만들고, 등록한 후, 생성된 제품을 반환한다.
  - createProduct() / registerProduct()
      - 하위 클래스에서 구현한다.
      - factory method 역할을 담당한다.  

```java
package framework;

public abstract class Factory {
  public final Product create(String owner) {
    Product p = createProduct(owner);
    registerProduct(p);
    return p;
  }
  protected abstract Product createProduct(String owner);
  protected abstract void registerProduct(Product product);
}
```  

</div>
</details>

<details>
**<summary><font color="blue">IDCard 클래스</font></summary>**
<div markdown="1">

**IDCard 클래스**
  - <font color="blue">idcard 패키지</font>
  - Product 클래스의 하위 클래스
  - 상위 클래스의 use() 메소드를 구현함
  - getOwner()를 추가함  

```java
package idcard;
import framework.*;

public class IDCard extends Product {
  private String owner;
  IDCard(String owner) {
    System.out.println(owner + "의 카드를 만듭니다.");
    this.owner = owner;
  }
  public void use() {
    System.out.println(owner + "의 카드를 사용합니다");
  }
  public String getOwner() {
    return owner;
  }
}
```  

</div>
</details>

<details>
**<summary><font color="blue">IDCardFactory 클래스</font></summary>**
<div markdown="1">

**IDCardFactory 클래스**
  - <font color="blue">idcard 패키지</font>
  - createProduct와 registerProduct를 구현
  - createProduct()
      - IDCard 제품을 실제로 생성함
      - 어떤 제품을 생산할 지 결정한다
  - registerProduct()
      - IDCard의 소유주를 owners 필드에 추가함(등록함)  

```java
package idcard;
import framework.*;
import java.util.*;

public class IDCardFactory extends Factory {
  private List owners = new ArrayList();
  protected Product createProduct(String owner) {
    return new IDCard(owner);
  }
  protected void registerProduct(Product product) {
    owners.add(((IDCard)product).getOwner()));
  }
  public List getOwners() {
    return owners;
  }
}
```  

</div>
</details>

<details>
**<summary><font color="blue">Main 클래스</font></summary>**
<div markdown="1">

**Main 클래스**
  - framework 패키지와 idcard 패키지를 이용해서 IDCard를 생성해서 사용함
  - 필요한 IDCard 공장을 만들고, IDCard 공장의 create() 메소드를 호출해서 원하는 IDCard 제품을 얻는다.  

```java
import framework.*;
import idcard.*;

public class Main {
  public static void main(String[] args) {
    Factory factory = new IDCardFactory();
    Product card1 = factory.create("홍길동");
    Product card2 = factory.create("이순신");
    Product card3 = factory.create("장성원");
    card1.use();
    card2.use();
    card3.use();
  }
}
```  

![](https://eliotjang.github.io/assets/images/system-analysis/ch04-2.png){: width="401" height="159"}  

</div>
</details>


## 04. Factory Method 패턴에 등장하는 역할  

**일반적인 클래스 다이어 그램**  

![](https://eliotjang.github.io/assets/images/system-analysis/ch04-3.png){: width="516" height="326"}  

**Product(제품)의 역할**
  - 생성된 제품(인스턴스)이 가지고 있어야 할 인터페이스(API)를 결정하는 추상 클래스
  - 구체적인 역할은 하위 클래스인 ConcreteProduct 역할이 결정한다.
  - 예제에서는, Product 클래스가 해당됨  

**Creator(생산자)의 역할**
  - Product 클래스를 생성하는 추상 클래스
  - Creator는 실제 제품을 생성하는 일을 하는 ConcreteCreator 역할에 대해서는 아무것도 모른다.
  - 예제에서는, Factory 클래스가 해당됨  

**ConcreteProduct(구체적 제품)의 역할**
  - 구체적인 제품을 나타내는 클래스
  - 예제에서는, IDCard 클래스가 해당됨  

**ConcreteCreator(구체적 생산자)의 역할**
  - 구체적인 제품을 만드는 클래스
  - 예제에서는, IDCardFactory 클래스가 해당됨  


## 05. 독자의 사고를 넓혀주는 힌트  

**프레임워크와 구체적인 공장 및 제품을 분리**
  - 같은 프레임워크를 이용해서(즉, framework 패키지 수정 없이), 다른 '공장'과 다른 '제품'을 추가로 정의할 수 있다.
      - 예: TV공장 + TV
  - framework 패키지 안에서는 idcard 패키지를 import 하지 않음
      - Product 클래스나 Factory 클래스 안에서는 IDCard나 IDCardFactory라는 구체적인 클래스 이름이 없음
      - 새로운 클래스를 동일한 framework에서 생성할 경우에도 <font color="blue">framework 패키지의 내용을 수정할 필요가 전혀 없음</font>
      - framework 패키지는 idcard 패키지에 <font color="red">의존하고 있지 않음</font>  

**예: TV공장 + TV**  

![](https://eliotjang.github.io/assets/images/system-analysis/ch04-4.png){: width="546" height="244"}  

  - <font color="red">Factory와 Product 수정 없이</font>, 다른 종류의 '제품'과 '공장'을 추가로 만들 수 있다.  
  ⇒ Main 클래스(클라이언트)에서는, TV 제품이 필요한 경우,  
  TVFactory 객체를 생성한 후에 TVFactory 객체의 create() 메소드를 호출하기만 하면 된다.  

**각 공장이 어떤 제품을 생산하는지에 대해서 클라이언트는 신경 쓰지 않는다(모른다)**
  - 예: IDCardFactory 공장이, IDCard 제품 대신에 IDCard2라는 제품을 생산하도록 바뀌어도, <font color="red">Main 클래스의 코드는 바뀌지 않는다</font>
      - 단지, IDCardFactory 객체를 생성하고, create() 메소드를 호출하기만 하면 된다.  

**인스턴스 생성 - 메소드 구현 방법**
  - Factory 클래스의 createProduct 메소드 구현 방법에는 3가지가 있다.  
  
  1. 추상 메소드로 한다.(예제 프로그램의 경우)
  2. 디폴트 구현을 준비해 둔다.
      - 하위 클래스에서 구현을 준비하지 않았을 경우에 이 디폴트 구현이 실행된다.  
      ```java
      class Factory {
	public Product createProduct(String name) {
	  return new Product(name);
	}
      }
      ```  

  3. 에러로 처리한다.
      - 디폴트 구현의 내용을 예외로 발생시키는 문장으로 한다.  
      ```java
      class Factory {
	public Product createProduct(String name) {
	  throw new FactoryMethodRuntimeException();
	}
      }
      ```  


## 06. Factory Method 패턴과 관련된 패턴  

**<font color="red">Template Method 패턴</font>(3장)**  
**Singleton 패턴(5장)**  
**Composite 패턴(11장)**  
**<font color="blue">Iterator 패턴</font>(1장)**
  - iterator() 메소드가, Iterator 인스턴스를 생성할 때, Factory Method 패턴을 사용하는 경우가 있다.  


## 07. 요약  

**제품 생성에 대한 프레임워크와 구체적인 공장 및 제품을 분리해서 구현한다.**
  - 제품이 필요한 경우, 클라이언트는 구체적인 공장에 대한 객체를 생성한 후 create 메소드를 호출하기만 하면 된다.  


## 연습문제(각자 공부할 것)  

**4-1**
  - IDCard 클래스의 생성자에 public이 붙어 있지 않은 이유는?  

**4-2. 프로그램 작성**
  - IDCard 제품 생성시 serial 번호를 붙인다.
  - 생상된 IDCard 제품의 owner와 serial 쌍을 저장한다.
      - HashMap 클래스 이용
  - IDCard 생산 방법이 바뀌어도, framework와 클라이언트(Main 클래스)쪽 코드는 변경이 없음에 주목해야 한다.  

**4-3**
  - 추상 클래스 정의 시, 생성자를 선언하면 어떻게 되는가?  






