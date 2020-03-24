---
title: "[웹데이터베이스] 2 week, HTML 태그 적용 및 문서 작성"
excerpt: "GWNU Computer Science & Engineering Lecture"

categories:
  - web database
tags:
  - web database
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

  > <font color=blue size=15>문서내용</font>  


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

**실습**  

윈도우
  1. 메모장으로 HTML 문서 작성  
  2. 저장  
    - 파일(F) → 저장(S)  
  3. 저장 시 중요사항  
    - 파일의 확장자는 『html』, 『파일형식』은 반드시 『모든파일』
    - 『인코딩』은 『UTF-8』 (글자가 깨질 경우 『ANSI』로 변경)  
  4. 윈도우에서 저장된 파일 더블 클릭  
  5. 웹 브라우저에서 확인



## Tag 종류 및 실습  

**BODY 태그**  

Body 영역 배경을 구성하는 태그(bgcolor, text 속성)  

실습(파일명: body.html)  



Body 영역 배경을 구성하는 태그(background 속성)  

실습(파일명: body-background.html)  
  - 첨부 파일 'tiger.jpg'를 body-background.html과 같은 폴더에 복사  


그 외의 BODY 태그 속성들  
  - link 속성 : 다른 html과 하이퍼링크로 연결된 부분의 최초 색상  
  - vlink 속성 : 하이퍼링크가 연결된 부분을 클릭한 후의 색상  
  - alink 속성 : 하이퍼링크가 연결된 부분을 누를 때 색상  
  - topmargin 속성 : 웹 페이지 body 부분의 상단 여백  
  - leftmargin 속성 : 웹 페이지 body 부분의 왼쪽 여백  

### 문장태그
**H 태그**  
  - <hn>문서 제목</hn>  
    - 문서 제목의 문자 크기 지정  
    - n은 1~6  

실습(파일명 : h.html)  

**p 태그**  
  - <p>문서 제목</p>  
    - 문장 정렬, align 속성을 사용하며 default 값은 left  
    - left, center, right 가능
실습(파일명 : p.html)  

**br과 hr태그**  
  - <br> : 줄바꾸기(Line Break)  
  - <hr> : 수평선 그리기  
    - `예: <hr size=2 color=red width=80% align=left noshade>`  

**center, pre 태그**  
  - <center>문장</center> : 문장을 가운데로 정렬  
  - <pre>텍스트</pre> : 문장 형식 그대로 출력  

실습(파일명 : center-pre.html)  

**font 태그**  
<font>텍스트</font> : 글자 크기, 색상 모양을 지정
  - size 속성: 글자 크기 지정  
  - color 속성: 글자 색상 지정  
  - face 속성: 글자체를 지정

**xmp, blockquote, comment 태그**  
<xmp>텍스트</xmp> : 예문을 나타낼 때 사용  
  - 글자 모양을 고정 폭 글자체 형식으로 출력  

<blockquote>텍스트</blockquote> : 글을 인용할 때 사용. 줄을 바꿔서 표시  

<!-- 주석 --> : 주석(comment)  
  - 주석 부분은 보이지 않음  

실습(파일명: xmp-blockquote-comment.html)  


### 논리적 스타일 태그  
**dfn, em, cite 태그**  

<dfn>용어 정의</dfn> : 용어 정의 태그  
  - 일반적으로 글자 모양은 이탤릭체 또는 볼드체  

<em> 기운 이탤릭체</em> : 문자 강조 때 사용하는 태그  
  - 글자가 기울어진 이탤릭체  

<cite>인용 문구</cite> : 책 또는 제목의 인용 문구를 나타낼 때 사용  
  - 일반적으로 이탤릭체  

<code>computer program code</code> : 컴퓨터 프로그램 코드를 나타낼 때 사용하는 태그  
  - 일반적으로 영어타자기체  

**kbd, samp, strike, strong 태그**  
<kbd>키보드</kbd> : computer로 입력한 글자를 나타낼 때 사용  
  - 일반적으로 고정 폭 글자체  
<samp>컴퓨터 상태 메시지</samp> : computer의 상태 메시지를 나타낼 때 사용  
  - 일반적으로 고정 폭 글자체  
<strike>강조 태그</strike> : 문장 강조 태그  
  - 문장 중간 부분에 줄을 그어 강조  
<strong>특정 문장 강조 태그</strong> : 특정 문장 강조  
  - bold체로 나타남  

**var, au, arg 태그**  
<var>변수이름</var> : 주로 변수 이름을 나타내는 태그  
  - 주로 Italic체  
<au>저자명</au> : 주로 저자(author)를 나타낼 때 사용하는 태그  
  - 일반적으로 고정 폭 글자체  
<arg>프로그램의 기능 및 줄거리</arg> : 프로그램의 기능 및 줄거리를 설명할 때 사용하는 태그  

**b, big, i, u 태그**  
<b>굵은 글씨체</b> : 원하는 글 부분을 굵은 글씨체로 나타내는 태그  
  - 굵은 글자체로 강조  
<big>큰 글씨체 태그</big> : 현재 정의된 폰트보다 좀 더 큰 글씨체를 만들 때 사용하는 태그  
  - 큰 글씨체로 강조  
<i>이탤릭 글씨체</i> : 이탤릭을 나타내는 태그  
  - 이탤릭 글씨체로 강조  
<u>특정 문자 아래에 밑줄</u> : 특정 문자에 underline 강조  
  - 밑줄로 강조  








