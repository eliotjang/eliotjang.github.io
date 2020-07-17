---
title: "HTML 태그 적용 및 문서 작성"
excerpt: "GWNU Computer Science & Engineering Lecture"

categories:
  - 컴퓨터공학
tags:
  - HTML

last_modified_at: 2020-03-24T17:00:00+09:00
---  

## HTML이란?  
  - HyperText Markup Language  
  - 웹 페이지 작성을 위한 언어  
  - 제목, 단락, 목록 등과 같은 문서의 구조적 의미를 나타내고, 링크 인용과 그 밖의 항목으로 문서들 간의 관계를 나타냄으로써 구조적 문서를 만들 수 있는 언어  


## Tag의 기본 형식  
문자열, 이미지, 소리, 동영상 같은 멀티미디어를 웹페이지에 나타내는 방법과 멀티미디어들간의 연결관계, 그리고 사용자 인터페이스를 제공하는 표시  

  1. `<tag>내용</tag>`  
  2. `<tag 속성=값>내용</tag>`  
  3. `<tag>`

> `예: <font color=blue size=15>문서내용</font>`  


## HTML의 기본 형식  

```html  
<!-- 문서의 시작부분 -->
<html>
  <!-- head의 시작부분 -->
  <head>
    <!-- 웹 문서 제목 -->
    <title>
      <!-- 문서 제목 -->
      웹 페이지 샘플
    <!-- 문서 제목 끝 tag -->
    </title>
  <!-- head 끝 tag -->  
  </head>
  <!-- 문서 내용 시작 tag -->
  <body>
    <!-- 문서 내용 부분 -->
    <font color=blue size=15>문서내용</font> 
  <!-- 문서 내용 끝 tag -->
  </body>
<!-- html 문서 끝 tag -->  
<html>
```  

윈도우(Window) 실습 방법
  1. 메모장으로 HTML 문서 작성  
  2. 저장  
    - 파일(F) → 저장(S)  
  3. 저장 시 중요사항  
    - 파일의 확장자는 『html』, 『파일형식』은 반드시 『모든파일』  
    - 『인코딩』은 『UTF-8』 (글자가 깨질 경우 『ANSI』로 변경)  
  4. 윈도우에서 저장된 파일 더블 클릭  
  5. 웹 브라우저에서 확인

맥(Mac) 실습 방법  
  1. Visual Studio Code(텍스트 편집기)로 HTML 문서 작성  
  2. 저장  
  3. Finder에서 저장된 파일 더블 클릭  
  4. 웹 브라우저에서 확인 

## Tag 종류 및 실습  


**BODY 태그**  
bgcolor, text 속성  

```html  
<html>
    <head>
        <title>
            BODY 태그
        </title>
    </head>
    <body bgcolor="gray" text="black">
        배경색과 글자색을 살펴보세요.
    </body>
</html>
```
[body.html](https://eliotjang.github.io/assets/images/web-database/body.html)  


background 속성  
```html  
<html>
    <head>
        <title>
            BODY 태그
        </title>
    </head>
    <body bgcolor="gray" text="black" background="rotc-eliot.jpeg">
        배경사진을 살펴보세요.
    </body>
</html>
```  
[body-background.html](https://eliotjang.github.io/assets/images/web-database/body.background.html)  

> 사진은 body-background.html과 같은 폴더에 있어야 한다.  

그 외 BODY 태그 속성들  
  - link 속성 : 다른 html과 하이퍼링크로 연결된 부분의 최초 색상  
  - vlink 속성 : 하이퍼링크가 연결된 부분을 클릭한 후의 색상  
  - alink 속성 : 하이퍼링크가 연결된 부분을 누를 때 색상  
  - topmargin 속성 : 웹 페이지 body 부분의 상단 여백  
  - leftmargin 속성 : 웹 페이지 body 부분의 왼쪽 여백  

- - -

**문장 태그**  
H 태그  
`<hn>문서 제목</hn>` : 문서 제목의 문자 크기 지정. n은 1~6  

```html  
<html>
    <title>
        H 태그
    </title>
    <body bgcolor="gray">
        <h1>h1 제목</h1>
        <h2>h2 제목</h2>
        <h3>h3 제목</h3>
        <h4>h4 제목</h4>
        <h5>h5 제목</h5>
        <h6>h6 제목</h6>
    </body>
</html>
```  
[h.html](https://eliotjang.github.io/assets/images/web-database/h.html)  

p 태그  
`<p>문서 제목</p>` : 문장 정렬, align 속성을 사용하며 default 값은 left  

```html  
<html>
    <head>
        <title>
            p 태그
        </title>
    </head>
    <body bgcolor="gray">
        <p align=left>왼쪽으로 정렬된 문장</p>
        <p align=center>가운데로 정렬된 문장</p>
        <p align=right>오른쪽으로 정렬된 문장</p>
    </body>
</html>
```
[p.html](https://eliotjang.github.io/assets/images/web-database/p.html)  

br, hr 태그  
  - `<br>` : 줄바꾸기(Line Break)  
  - `<hr>` : 수평선 그리기  
    > size, width, align의 값에 double quotes(쌍따옴표)를 넣거나  
    > noshade를 noshade="noshade" 형식으로 바꿔도 상관없다.  

```html  
<html>
    <head>
        <title>
            br, hr 태그
        </title>
    </head>
    <body bgcolor="green">
        아무리 enter를 쳐도 
        줄이 
        바꾸지 않습니다.
        <hr>
        이렇게 hr를 이용하여 수평선을 그리거나<br>
        br을 이용하여<br>
        줄을 바꿀 수 있습니다. <hr size=5 width=80% align=right noshade>
    </body>
</html>
```  
[br-hr.html](https://eliotjang.github.io/assets/images/web-database/br-hr.html)  

center, pre 태그  
  - `<center>문장</center>` : 문장을 가운데로 정렬  
  - `<pre>텍스트</pre>` : 문장 형식 그대로 출력  

```html  
<html>
    <head>
        <title>
            center-pre 태그
        </title>
    </head>
    <body bgcolor="gray">
        <center>center 태그는 문장을 가운데로 정렬한다.</center>
        <pre>
            pre 태그는

            문장을
            표현하는 그대로

            출력한다...
        </pre>
    </body>
</html>
```  
[center-pre.html](https://eliotjang.github.io/assets/images/web-database/center-pre.html)  
  
font 태그  
`<font>텍스트</font>` : 글자 크기, 색상 모양을 지정
  - size 속성: 글자 크기 지정  
  - color 속성: 글자 색상 지정  
  - face 속성: 글자체를 지정  

```html  
<html>
    <head>
        <title>
            font 태그
        </title>
    </head>
    <body bgcolor="gray">
        <font size="5" color="green" face="바탕">
            font는 글자의 크기, 색상, 글자체를 정의합니다.
        </font>
    </body>
</html>
```  
[font.html](https://eliotjang.github.io/assets/images/web-database/font.html)  


**xmp, blockquote, comment 태그**  
`<xmp>텍스트</xmp>` : 예문을 나타낼 때 사용. 글자 모양을 고정 폭 글자체 형식으로 출력  
`<blockquote>텍스트</blockquote>` : 글을 인용할 때 사용. 줄을 바꿔서 표시  
`<!-- 주석 -->` : 주석(comment). 주석 부분은 보이지 않음  

```html  
<html>
    <head>
        <title>
            xmp, blockquote, comment 태그
        </title>
    </head>
    <body bgcolor="gray">
        xmp 앞: <xmp> 이 부분이 xmp를 사용한 문장입니다.</xmp> xmp 뒤 <hr>
        blockquote 앞: <blockquote> 이 부분이 blockquote를 사용한 문장입니다.</blockquote> blockquote 뒤 <hr>
        주석 태그 앞: <!-- 주석 --> 주석 태그 뒤 <hr>
    </body>
</html>
```  
[xmp-blockquote-comment.html](https://eliotjang.github.io/assets/images/web-database/xmp-blockquote-comment.html)  

### 【논리적 스타일 태그】  

**dfn, em, cite, code 태그**  
`<dfn>용어 정의</dfn>` : 용어 정의 태그. 일반적으로 글자 모양은 이탤릭체 또는 볼드체  
`<em> 기운 이탤릭체</em>` : 문자 강조 때 사용하는 태그. 글자가 기울어진 이탤릭체  
`<cite>인용 문구</cite>` : 책 또는 제목의 인용 문구를 나타낼 때 사용. 일반적으로 이탤릭체  
`<code>computer program code</code>` : 컴퓨터 프로그램 코드를 나타낼 때 사용하는 태그. 일반적으로 영어타자기체  

```html  
<html>
    <head>
        <title>
            dfn, em, cite, code 태그
        </title>
    </head>
    <body bgcolor="gray">
        1. dfn 태그 : <dfn>용어 정의. 일반적으로 글자 모양은 이탤릭체 또는 볼드체</dfn><br>
        2. em 태그 : <em>글자를 기울어지게 하는 이탤릭체로 문자 강조 때 사용</em><br>
        3. cite 태그 : <cite>책 또는 제목의 인용문구를 나타낼 때 사용. 일반적으로 이탤릭체</cite><br>
        4. code 태그 : <code>computer program code를 나타낼 때 사용. 일반적으로 영어 타자기체</code>
    </body>
</html>
```
[dfn-em-cite-code.html](https://eliotjang.github.io/assets/images/web-database/dfn-em-cite-code.html)  

**kbd, samp, strike, strong 태그**  
`<kbd>키보드</kbd>` : computer로 입력한 글자를 나타낼 때 사용. 일반적으로 고정 폭 글자체  
`<samp>컴퓨터 상태 메시지</samp>` : computer의 상태 메시지를 나타낼 때 사용. 일반적으로 고정 폭 글자체  
`<strike>강조 태그</strike>` : 문장 강조 태그. 문장 중간 부분에 줄을 그어 강조  
`<strong>특정 문장 강조 태그</strong>` : 특정 문장 강조. bold체로 나타남  

```html  
<html>
    <head>
        <title>
            kbd, samp, strike, strong 태그
        </title>
    </head>
    <body bgcolor="gray">
        5. kbd 태그 : <kbd>computer로 입력한 글자를 나타낼 때 사용. 일반적으로 고정 폭 글자체</kbd><br>
        6. samp 태그 : <samp>computer의 상태메시지를 나타낼 때 사용. 일반적으로 고정 폭 글자체</samp><br>
        7. strong 태그 : <strong>특정 문장을 강조하는 태그. bold체로 나타남</strong><br>
        8. strike 태그 : <strike>문장 중간 부분에 줄을 그어 강조하는 태그</strike>
    </body>
</html>
```  
[kbd-samp-strike-strong.html](https://eliotjang.github.io/assets/images/web-database/kbd-samp-strike-strong.html)  

**var, au, arg 태그**  
`<var>변수이름</var>` : 주로 변수 이름을 나타내는 태그. 주로 Italic체  
`<au>저자명</au>` : 주로 저자(author)를 나타낼 때 사용하는 태그. 일반적으로 고정 폭 글자체  
`<arg>프로그램의 기능 및 줄거리</arg>` : 프로그램의 기능 및 줄거리를 설명할 때 사용하는 태그  

```html  
<html>
    <head>
        <title>
            var, au, arg 태그
        </title>
    </head>
    <body bgcolor="gray">
        9. var 태그 : <var>주로 변수 이름을 나타내는 태그. 주로 Italic체</var><br>
        10. au 태그 : <au>주로 글쓴이(author)를 나타낼 때 사용하는 태그</au><br>
        11. arg 태그 : <arg>프로그램의 기능 및 줄거리를 설명할 때 사용하는 태그</arg>
    </body>
</html>
```  
[var-au-arg.html](https://eliotjang.github.io/assets/images/web-database/var-au-arg.html)  


**b, big, i, u 태그**  
`<b>굵은 글씨체</b>` : 원하는 글 부분을 굵은 글씨체로 나타내는 태그  
`<big>큰 글씨체 태그</big>` : 현재 정의된 폰트보다 좀 더 큰 글씨체를 만들 때 사용하는 태그  
`<i>이탤릭 글씨체</i>` : 이탤릭을 나타내는 태그  
`<u>특정 문자 아래에 밑줄</u>` : 특정 문자에 underline 강조  

```html  
<html>
    <head>
        <title>
            b, big, i, u 태그
        </title>
    </head>
    <body bgcolor="gray">
        1. b 태그 : <b>원하는 글 부분을 굵은 글씨체로 나타내는 태그</b><br>
        2. big 태그 : <big>현재 정의된 폰트보다 좀 더 큰 글씨체를 만들 때 사용되는 태그</big><br>
        3. i 태그 : <i>이탤릭을 나타내는 태그</i><br>
        4. u 태그 : <u>특정 문자에 underline</u>
    </body>
</html>
```  
[b-big-i-u.html](https://eliotjang.github.io/assets/images/web-database/b-big-i-u.html)  


