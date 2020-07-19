---
title: "[alogrithm]Difference between a **let** and a **var**"
excerpt: "About JavaScript"

categories:
  - algorithm
tags:
  - javascript

last_modified_at: 2020-05-08T18:00:00+09:00
---

## Question

- What is the different between using "let" and "var"?


## 1st Answer

Main difference is scoping rules. Variables declared by `var` keyword are scoped to the immediate function body (hence the function scope) while `let` variables are scoped to the immediate *enclosing* block denoted by `{ }` (hence the block scope).  

## 2st Answer (Korean Language)

`let` 구문은 블록 유효 범위를 갖는 지역 변수를 선언하며, 선언과 동시에 임의의 값으로 초기화할 수도 있다.  

`let`은 변수가 선언된 블록, 구문 또는 표현식 내에서만 유효한 변수를 선언한다. 이는 `var` 키워드가 블록 범위를 무시하고 전역 변수나 함수 지역 변수로 선언되는 것과 다른 점이다.

### 유효 범위 규칙

`let`으로 선언된 변수는 변수가 선언된 블록 내에서만 유효하며, 당연하지만 하위 블록에서도유효하다. 이러한 점에서는 `let`과 `var`는 함수 블록 이외의 블록은 무시하고 선언된다는 점에 다르다.  


## References(참고 문헌)

- <https://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var>
- <https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let>  


