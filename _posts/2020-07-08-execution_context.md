---
title: 'Execution Context'
toc: true
toc_sticky: true
categories:
   - javascript
last_modified_at: 2020-06-16T08:06:00-05:00
---

## Executable Code

javascript engine은 실행가능한 code를 만나면 그 code를 평가해서 Execution Context(실행 문맥)을 만듦.

### 실행 가능한 코드의 유형

1. 전역 코드 : window 전역 객체 아래 정의된 함수.
2. 함수 코드
3. eval code : eval 함수(lexical Environment가 아닌 별도의 동적 환경에서 실행됨.)

### 코드를 실행하기 위한 환경

1. 변수 : 전역변수, 지역변수, 매개변수, 함수의 선언
2. arguments 객체
3. scope
4. this

# Execution Context

실행가능한 코드가 실제로 실행되고 관리되는 영역.  
실행에 필요한 모든 compoenent를 여러개로 나누어 관리한다.
Stack 구조로 관리됨. (call stack)

```
                                   Execution Context
                             ┌─────────────┴─────────────┐
                    Lexical Environment        Variable Environment
                   ┌────────┴─────────────────────────┐
            Environment Record         Outer Lexical Environment Referance
          ┌────────┴──────────────────────────┬─────────────────────────┬───────────────────────────────┐
   Declarative Environment Record   Object Environment Record     Global Environment Record       This Binding
          ┌────────┴─────────────────────────┐
*Function Environment Record     Module Environment Record
```

## Lexical Environment

javascirpt engine이 code를 실행하기 위해 자원을 모아 둔 곳.  
함수 또는 block의 유효범위 안에 있는 식별자와 그 결과값이 저장.
javascript engine은 유효범위 안의 식별자와 결과값을 바인드해서 Environment Record에 기록.

## Variable Environment

변수 환경 ... 자료 미흡.

## Lexical Environment

어휘 환경.  
javascript engine이 code를 실행하기 위해 자원을 모아 둔 곳.  
Function 또는 Block의 유효범위 안에 있는 식별자와 그 결과값이 저장.  
javascript engine은 유효범위안에있는 식별자와 그 식별자가 가리키는 값을 key-value로 Bind.

### Outer Lexical Environment Referance

외부 어휘 환경 참조.  
javascript는 함수안에 함수를 중첩해서 정의 할 수도 있다.  
유효범위 너머의 유효범위도 검색할수 있다.  
함수를 둘러싼 코드가 속한 lexical environment referance가 저장됨.

중첩된 함수 안에서 바깥코드의 정의된 변수를 읽거나 사용할 때 outer lexical environment referance를 따라 변수를 검색.

javascript는 lexical scope를 갖는 언어이다.  
식별자 탐색에 있어서 당연히 scope chain도 포함.  
outer는 scope chain을 위해 존재하는 참조라고 할 수 있다.
ECMAScript3까지는 scope chain이라는 용어를 썻지만 ECMAScript5부터는 `lexical nesting structure` or `logical nesting of lexical Environment valuse`라는 용어를 사용.

### Environment Record

Identifier Bindings. 식별자들의 바인딩을 기록하는 객체 (변수, 함수등이 기록되는 곳.)

#### Global Environment

javscirpt interperter는 시작하자마자 lexical environment `Type`의 Global Environmnet를 생성.  
전역 객체를 생성한 다음 Global Environment의 Object Environment Record에 전역 객체의 참조를 대입함.

> var type <br/>최상위 level의 var로 작성된 변수는 Global Environment의 property로 추가됨. <br/>`hoisting` 최상위 레벨에 선언된 변수와 함수는 프로그램을 평가하는 시점에 Objecto Environment Record에 추가된 상태이기 때문에 어느위치에서도 접근 가능.<br/>var문과 함수 선언문으로 선언한 전역변수는 [[Configable]]속성이 `false`이기 때문에 `delete`연산이 불가.<br/>

#### This Binding

함수를 호출한 객체의 참조가 저장된다.
Execution Context의 This.
ECMAScript5까지는 This Binding을 Execution Context에서 관리했다면, ES6부터는 Envirionment Record에서 관리.
Record가 식별자들의 정보를 관리하는 객체기때문에 더 적합한듯.

#### Object Environment Record

Execution Context 외부에 별도로 저장된 객체의 참조에서 데이터를 읽거나 사용.
lexical environment나 전역객체처럼 별도로 저장된 데이터는 key-value를 복사하는 것이 아닌 해당 객체의 참조를 가져와서 Object environment record의 bindObject라는 property에 bind하여 사용함.

#### Declarative Environment Record

선언적 환경 레코드.  
실제로 함수, 변수, catch문의 식별자와 실행결과가 저장.

##### Function Environment Record

NewFunctionEnvironment()에서 언급한 `new.target`, `this`, `super` 등에 대한 정보를 갖는다.

##### Module Environment Record

모듈 관련 정보를 갖고있지 않을까?
