---
title: "[algorithm]Difference between an **Array**, **ArrayList** and a **List**"
excerpt: "with C#"

toc: true
toc_sticky: true

categories:
  - algorithm
tags:
  - C#
  - Stack Overflow

last_modified_at: 2020-06-05T19:00:00+09:00
---

## Question

- What is the different between an "Array", "ArrayList" and a "List"?

## Example  

**Array**  
For the **Array** we can only add types that we declare for this example an **int**.  

```cs
int[] Array = new Int[5];
for(int i = 0; i < Array.Length; i++)
{
  Array[i] = i + 5;
}
```  


**ArrayList**  
We can add values just like a **Array**  

```cs
ArrayList arrayList = new ArrayList();
arrayList.Add(6);
arrayList.Add(8);
```  


**List**  
Again we can add valuees like we do in **Array**  

```cs
List<int> list = new List<int>();
list.Add(6);
list.Add(8);
```  



## 1st. Answer  

They are different object types. They have different capabilities and store their data in different ways. You may as well ask what is different between a decimal and a DateTime.  


An Array(`System.Array`) is fixed in size once it is allocated. You can't add items to it or remove items from it. Also, all the elements must be the same type. As a result, it is type safe, and is also the most efficient of the three, both in terms of memory and performance. Also, System.Array supports multiple dimensions. (i.e. it has Rank property) while List and ArrayList do not (although you can create a List of Lists or an ArrayList of ArrayLists, if you want to).  


An `ArrayList` is a flexible array which contains a list of objects You can add and remove items from it and it automatically deals with allocating space. If you store value types in it, they are boxed and unboxed, which can be a bit inefficient. Also, it is not type-safe.  


A `List<>` leverages generics; it is essentially a type-safe version of ArrayList. This means there is no boxing or unboxing (which improves performance) and if you attempt to add an item of the wrong type it'll generate a compile-time error.  


## 2nd. Answer (한글 번역)  

|Array|ArrayList|List|
|-----|---------|----|
|고정된 배열 크기를 갖는다.(선언 시 크기를 지정해주고, 삭제 및 추가와 같은 변형이 불가능하다.)<br/>같은 타입만 저장 가능하다(type-safe하다)<br/>다차원 배열 입력이 가능하다|고정되지 않는, 추가/삭제의 변형이 가능한 객체 타입이다<br/>제네릭 타입으로서 서로 다른 타입의 데이터가 배열에 저장 가능하다. 때문에 데이터를 가져올 때 박싱, 언박싱이 발생하며, type-safe하지 못한 이슈가 있다|마찬가지로 고정되지 않는 가변 객체 타입이다<br/>ArrayList의 단점을 보완하여 컴파일시 배열의 타입추론을 한다. 즉, 같은 타입만 저장가능하고, 때문에 박싱/언박싱이 발생하지 않는다|  


array와 arraylist는 같은 배열이지만 서로 다른 특성을 가지고 있고, 두 배열의 타입이 정반합이 list라고 이해하면 될 것 같다.  



## References(참고 문헌)

- <https://stackoverflow.com/questions/32020000/what-is-the-difference-between-an-array-arraylist-and-a-list/32020072>  
- <https://dev-yeon.tistory.com/8>  


