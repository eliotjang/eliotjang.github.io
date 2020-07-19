---
title: "[디자인 패턴] Chapter 23. Interpreter 패턴" 
excerpt: "Hiroshi Yuki, Java 언어로 배우는 디자인 패턴 입문"  

toc: true
toc_sticky: true
toc_label: "Ch23. Interpreter 패턴"
header:
  teaser: /assets/images/system-analysis/system-analysis-logo.jpeg
categories: 
  - 컴퓨터공학
tags:
  - 디자인-패턴
  - java

last_modified_at: 2020-06-23T23:00:00+09:00  
---  

## 01. Interpreter 패턴  

- 프로그램이 해결하려고 하는 문제를 간단한 '미니 언어'로 표현함 
  - 구체적인 문제를 미니 언어로 쓰여진 '미니 프로그램'으로 표현한다 
  - 미니 프로그램을 자바 언어로 '통역'하는 역할을 하는 프로그램 
  - 통역 프로그램은 미니 언어를 이해하고 미니 프로그램을 해석 및 실행한다 
  - 해결해야 할 문제에 변화가 생겼을 때, 프로그램을 고치지 않고 미니 프로그램을 고쳐서 해결한다  

- Interpreter 패턴 적용 시, 해결하고자 하는 문제에 변화가 생겼을 때 미니 언어로 쓰여진 프로그램만 수정하면 된다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch23-1.png){: width="80%"}  



## 02. 미니 언어  

- "미니 언어'로 작성된 명령 
  - 예 : 무선 조정기로 자동차 움직이기 
    - 자동차에 내릴 수 있는 명령 
      - 앞으로 1미터 전진(go) / 우회전(right) / 좌회전(left)
      - 반복(repeat)
      - 좌/우회전은, 제자리에서 회전하는 것으로 가정함  
![](https://eliotjang.github.io/assets/images/system-analysis/ch23-2.png){: width="50%"}  


- 미니 언어로 작성된 미니 프로그램의 예 
  - program go end  
![](https://eliotjang.github.io/assets/images/system-analysis/ch23-3.png){: width="100%"}  
  - program go right go right go right go right end  
![](https://eliotjang.github.io/assets/images/system-analysis/ch23-4.png){: width="100%"}  
  - program repeat 4 go right end end  
![](https://eliotjang.github.io/assets/images/system-analysis/ch23-5.png){: width="50%"}  
  - program repeat 4 repeat 3 go right go left end right end end  
![](https://eliotjang.github.io/assets/images/system-analysis/ch23-6.png){: width="100%"}  

<br/>
- 미니 언어의 문법 
  - BNF(Backus-Naur Form 또는 Backus Normal Form)의 확장으로 문법을 표기함  
![](https://eliotjang.github.io/assets/images/system-analysis/ch23-7.png){: width="100%"}  
  - `<program> ::= program <command list>`
    - `<program>이란 program이라는 단어 뒤에 <command list>가 이어진 것`이라는 정의를 나타냄 
  - `<command list> ::= <comand>*end`
    - `<command list>는, <command>가 0개 이상 반복된 후 end라는 단어가 온것`이라는 뜻
  - `<command> ::= <repeat command> | <primitive command>`
    - `<command>란, <repeat command> 또는 <primitive command> 둘 중 하나`라는 뜻
  - `<repeat command> ::= repeat <number><command list>`
    - `<repeat command>란, repeat라는 단어 뒤에 반복횟수 <number>가 이어지고, 다시 <command list>가 이어진 것`이라는 뜻 
    - 그런데, `<command list>`는 이미 정의되어 있다 
      - `<command list>`정의 안에 `<command>`가 사용되고,
      - `<command`정의 안에 `<repeat command>`가 사용되고,
      - `<repeat command>`정의 시, `<command list>`가 사용된다 
      - ⇒ 즉, `<command list>`정의 시, `<command list>`가 다시 등장한다 
    - '재귀적인 정의' : 어떤 것을 정의하는데 자기 자신이 등장하는 경우 
  - `<primitive command> ::= go | right | left`
    - `<primitive command>란, go 또는 right 또는 left 이다`라는 뜻 
  - `<number>`는, 숫자로 이루어진 자연수를 나타냄  

<br/>
- terminal expression과 non-terminal expression
  - terminal expression
    - 문법 규칙에서, 더 이상 전개되지 않는 expression
    - 문법 규칙의 종착점을 의미함
    - 예 : `<primitive command>`
  - nont-terminal expression
    - 문법 규칙에서, 계속해서 다시 전개되는 expression
    - 예 : `<program>` 또는 `<command>`  


## 03. 예제 프로그램  

- 2절의 미니 언어를 해석하는 프로그램 
  - 문자열로 이루어진 미니 프로그램을 분해해서, 각 부분이 어떤 구조로 되어 있는지 해석한다 
  - 이를, 구문 분석(syntax analysis)이라고 한다 
  - 결과로, 구문 트리(syntax tree)가 만들어진다 
  - 예 : 3*5+2  
![](https://eliotjang.github.io/assets/images/system-analysis/ch23-8.png){: width="50%"}  
  - 예 : "program repeat 4 go right end end"라는 미니 프로그램이 주어지면, 구문 분석을 통해 다음과 같은 구문 트리(syntax tree)가 만들어진다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch23-9.png){: width="80%"}  
  - 예제 프로그램은, "program repeat 4 go right end end"를 해석하여 다음과 같은 구조를 메모리 상에 만든다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch23-10.png){: width="80%"}  


|이름|해설|
|----|----|
|Node|구문 트리의 '노드'가 되는 클래스|
|ProgramNode|`<program>`에 대응하는 클래스|
|CommandListNode|`<command list>`에 대응하는 클래스|
|CommandNode|`<command>`에 대응하는 클래스|
|RepeatCommandNode|`<repeat command>`에 대응하는 클래스|
|PrimitiveCommandNode|`<primitive command>`에 대응하는 클래스|
|Context|구문해석을 위한 전후 관계를 나타내는 클래스|
|ParseException|구문해석 중의 예외 클래스|
|Main|동작 테스트용 클래스|  

![](https://eliotjang.github.io/assets/images/system-analysis/ch23-11.png){: width="100%"}  


- 프로그램 작동 방식 
  - 예 : program go right go left end
    - 먼저, ProgramNode의 parse()는,
      - "program"이라는 단어를 확인한 후에,
      - CommandListNode에게 나머지 부분을 parse 하도록 한다 
    - CommandListNode의 parse()는,
      - "end"라는 단어가 나올 때까지 CommandNode에게 parse()를 시킨다 
    - CommandNode의 parse()는,
      - 현재 토큰이 "repeat"라는 단어이면, RepeatCommandNode에 parse()를 부탁하고,
      - 그렇지 않으면, PrimitiveCommandNode에게 parse()를 부탁한다 
    - PrimitiveCommandNode의 parse()는,
      - "go"나 "right"나 "left"가 아니면, 예외를 발생시킨다  


### Node 클래스  

- 구문 트리의 각 부분(노드)를 구성하는 최상위의 클래스 
- 추상 메소드 Parse(Context)
  - 구문 분석이라는 처리를 실행하기 위한 메소드 
  - 입력 인자 Context는, 현재 구문 분석을 실행하고 있는 '상황'을 나타내는 클래스 
  - 구문 분석 중에 에러가 발생하면, ParseException을 던진다  

### ProgramNode 클래스  

- `<program> ::= program <command list>`에서 `<program>`을 나타냄 
- Node commandListNode 필드 
  - 자신의 뒤에 이어지는 `<command list>`에 대응하는 구조(노드)를 보관함 
- 구문 분석 시 처리의 기본 단위를 '토큰(token)'이라고 한다 
  - 예를 들면, 3+5는 세 개의 토큰으로 나누어진다 
- 입력 문자열을 토큰으로 분리시키는 작업을 '어휘분석(lexical analysis)'이라고 한다 
- 토큰들부터 구문 트리르 만드는 작업을 구문 분석(parse)이라고 한다 
- parse()
  - 'program'이라는 토큰이 나오면 건너뛴다 
  - 다음으로, `<command list>`에 대응하는 CommandListNode의 인스턴스를 생성하고 그 인스턴스의 parse()를 호출한다 
- toString()
  - 노드를 문자열로 표시하는 메소드  


### CommandListNode 클래스  

- `<command lsit> ::= <command>*end`에서 `<command list>`을 나타냄 
- Vector list 필드 
  - 0번 이상 반복하는 `<command>`를 보관함 
  - `<command>`에 대응하는 CommandNode 클랫의 인스턴스르 넣어둠 
- parse()
  - 남은 토큰이 없다면(null) ⇒ 'end'가 빠졌다는 예외를 발생시킴 
  - 현재 토큰이 'end'라면 ⇒ 'end'를 건너띄고 while 루프를 break함 
  - 현재 토큰이 'end'가 아니라면 ⇒ 이 때는 `<command>`를 의미하므로, CommandNode의 인스턴스를 만들어서 그것의 parse()를 호출한 후, list에 추가한다  


### CommandNode 클래스  

- `<command> ::= <repeat command> | <primitive command>`에서 `<command>`를 나타냄 
- node 필드 
  - RepeatCommandNode 또는 PrimitiveCommandNode의 인스턴스를 넣어두는 변수 
- parse()
  - 현재 토큰이 "repeat"이면, RepeatCommandNode 인스턴스를 생성한 후 그것의 parse()를 호출한다 
  - 그렇지 않으면, PrimitiveCommandNode 인스턴스를 생성한 후 그것의 parse()를 호출한다  


### RepeatCommandNode 클래스  

- `<repeat command> ::= repeat <number> <command list>`에서 `<repeat command>`를 나타냄 
- number 필드 : `<number>` 부분이 저장됨 
- commandListNode 필드 : `<command list>` 부분이 저장됨 
- parse() : 재귀성을 가진다  

![](https://eliotjang.github.io/assets/images/system-analysis/ch23-12.png){: width="100%"}  



### PrimitiveCommandNode 클래스  

- `<primitive command> ::= go | right | left`에서 `<primitive command>`를 나타냄 
- parse()
  - 다른 parse() 메소드를 호출하지 않는다 
  - 다음 토큰을 얻은 후 건너뛴다 
  - 얻은 토큰이 "go", "right", "left" 중에 하나가 아니면, 예외를 발생시킨다  


### Context 클래스  

- 구문 분석을 위해 필요한 메소드를 제공한다 
  - 미니 프로그램을 유지하면서, 필요할 때마다 토큰으로 잘라서 그 토큰을 반환하는 일을 함 
  - 
|이름|해설|
|----|----|
|nextToken|다음의 토큰을 얻습니다(다음의 토큰으로 나아갑니다)|
|currentToken|현재의 토큰을 얻습니다(다음의 토큰으로 나아가지 않습니다)|
|skipToken|현재의 토큰을 체크하고 나서 다음의 토큰을 얻습니다(다음의 토큰으로 나아갑니다)|
|currentNumber|현재의 토큰을 수치로서 얻습니다(다음의 토큰으로 나아가지 않습니다)|  

- java.util.StringTokenizer를 이용한다 
  - 주어진 문자열을 토큰으로 분할해 주는 클래스 
  - 구분 문자(delimiter) : 스페이스, 탭, 뉴라인, 캐리지리턴, 폼피드 
  - 
|이름|해설|
|----|----|
|nextToekn|다음의 토큰을 얻습니다(다음의 토큰으로 나아갑니다)|
|hasMoreTokens|다음의 토큰이 있는지 없는지 조사합니다|  

- skipToken()
  - 현재 토큰과 입력인자 token을 비교해서,
    - 같으면 다음 토큰으로 진행하고
    - 다르면, 예외를 발생시킨다 
- currentNubmer()
  - 현재 토큰 문자열을 정수로 바꾸어주는 메소드  

### ParseException 클래스  

- 구문 분석 중에 발생한 예외를 위한 클래스  

### Main 클래스  

- 미니언어의 인터프리터를 작동시키기 위한 클래스 
- program.txt 파일의 내용을 한 줄씩 읽어서 구문 분석을 시킨다 
- 그리고 나서, 그 결과를 문자열로 화면에 출력한다  

![](https://eliotjang.github.io/assets/images/system-analysis/ch23-13.png){: width="90%"}  


## 04. 등장 역할  

- AbstractExpression(추상적인 수식) 역할 
  - 구문 트리의 노드가 가지는 공통적인 인터페이스(API)를 정하는 역할 
  - 예제에서는 Node 클래스가 해당됨 
- TerminalExpression(종착점 수식)의 역할 
  - BNF의 터미널 익스프레션에 대응하는 역할 
  - 예제에서는 PrimitiveCommandNode 클래스가 해당됨 
- NonterminalExpression(논터미널 수식)의 역할 
  - BNF의 논터미널 익스프레션에 대응하는 역할 
  - ProgramNode, CommandNode, RepeatCommandNode, CommandListNode가 해당됨 
- Context(문맥, 전후관계)의 역할 
  - 인터프리터가 구문 분석을 수행하도록 정보를 제공하는 역할 
  - 예제에서는 Context 클래스가 해당됨 
- Client(의뢰자)의 역할 
  - 구문트리를 조립하기 위해, TerminalExpression이나 NontermianlExpression을 호출하는 역할 
  - 예제에서는 Main 클래스가 해당됨  

![](https://eliotjang.github.io/assets/images/system-analysis/ch23-14.png){: width="100%"}  


## 05. 사고를 넓혀주는 힌트  

- 그 밖의 미니 언어 
  - 정규 표현(regular expression)
    - describes a pattern or sequence of characters
    - 문자들의 패턴이나 조합을 기술하는 표현식 
    - 예 : raining & (dogs | cats) *
      - raining 뒤에 dogs이나 cats가 0번 이상 반복되는 패턴을 기술한 식 
  - 검색용 수식 
    - 검색 시, 단어의 조합을 표현하기 위한 언어에 사용 가능하다 
    - 예 : garlic and not onions
      - garlic은 포함하지만, onions는 포함하지 않는 검색식을 나타냄  


## 06. 관련 패턴  

- Composite 패턴 
- Flyweight 패턴 
- Visitor 패턴  


## 07. 요약  

- 미니 언어를 사용해서 문제를 해결하는 Interpreter 패턴  


## 연습 문제  

- 23-1
  - 구문 분석 후, 기본 커맨드인 go, right, left에 해당하는 일을 하는, 실제로 수행하는 프로그램을 작성하기 
  - 특징 
    - GUI를 사용해서 기본 커맨드의 결과를 화면에 직접 그린다 
    - Facade 패턴을 이용해서, 인터프리터를 이용하기 쉽게 함 
    - 기본 커맨드를 생성하는 클래스를 생성하는 공장 클래스를 작성했다(4장의 Factory 패턴 이용)
    - 인터프리터 부분을 다른 패키지에 넣음으로써, GUI 부분과 분리했다 
  - 동작방식 
    - 사용자가 GUI의 textfield에 미니 프로그램을 입력한 후 enter를 치면 ⇒ Main의 actionPerformed()가 실행된다 
    - Main의 actionPerformed()에서는, parseAndExecute()를 호출한다 
    - parseAndExecute()는, 미니 프로그램을 파싱한 후, repaint()를 호출하여, 자동으로 paint()가 실행되도록 한다 
    - paint()에서는, InterpeterFacade의 execute()를 호출한다 
    - InterpreterFacade의 execute()는, 파싱 시에 생성된 ProgramNode의 execute()를 호출한다 
    - ProgramNode의 execute()은, CommandListNode의 execute()을 호출한다 
    - 결국, PrimitiveNode의 execute()이 호출이 되어서, 해당 Executor의 execute()를 호출해서, 캔버스에 실제 자동차의 움직임을 그린다 
  - InterpreterFacade
    - 기존의 인터프리터 관련 클래스들을 사용하기 좋게 하기 위한 창구 역할을 한다 
  - Main의 main()는,
    - InterpreterFacade 객체만을 이용해서 인터프리터를 사용한다  
![](https://eliotjang.github.io/assets/images/system-analysis/ch23-15.png){: width="80%"}  
  - 구문 트리의 각 Node는,
    - execute() 메소드가 추가된 것만 다르고, 나머지는 예제 프로그램과 같다 
    - 단, PrimitiveCommandNode의 parse() 메소드에서, 파싱 시 해당 Executor를 생성하는 문장이 추가되었다 
  - 해당 Executor는, ExecutorFactory를 구현한 TurtleCanvas가 생성된다 
    - createExecutor() 참조 
      - "go"인 경우에는 GoExecutor를, "right"/"left"인 경우에는 DirectionExecutor를 생성한다 
      - 이 메소드의 내용을 수정하면, 다른 일을 하는 Executor를 생성하도록 바꿀 수 있다(Factory 패턴을 사용한 이유가 여기에 있다)  


















