---
title: 'function에 관하여'
toc: true
toc_sticky: true
categories:
   - javascript
last_modified_at: 2020-06-16T08:06:00-05:00
---

# Function Object

ECMAScript에서 함수객체란 subroutine으로 수행 될 수 있는 객체.  
동작을 나타내는 실행 code와 상태를 포함하고 있음.  
객체 지향의 생성자 역할도 가능
일반적인(ordinary object`Internal slot + Internal method 모두 보유`)객체와 동일한 동작 가능.
Function = 일반적인 객체의 확장 + 함수로 동작하기 위한 추가적 기능

1. 클로저로 묶이는 Lexical Environment -> `[[Environment]]`
2. 함수 코드 - `[[ECMAScriptCode]]`
3. 함수 종류 - `[[FunctionKind]]`
   `normal`, `classConstructor`, `generator`, `async`
4. 생성자 종류 - `[[constructorKind]]`
   `base`, `derired`
5. this 참조형태 - `[[Thismode]]`
6. strict mode 여부 - `[[Strict]]`
7. super 참조 - `[[HiomeObject]]`
8. 기타

## 함수 생성시 기본적으로 사용되는 6가지 정보

1. 함수 생성 방식(종류)
   `Normal`, `Arrow`, `Method`
2. 함수의 매개변수 List
3. 함수 몸체 (함수 code)
4. scope(Lexical Environment)
5. strict mode 여부
6. 함수 객체의 ProtoType
   `Function Prototype`, `Generator`, `Async Function Prototype`

## 함수가 생성 될때

1. 함수생성방식을 통해 생성자가 될 수 있는지 판단
2. `strict`여부와 실제 함수객체의 종류(일반 generator, async ...)를 구분하여 저장.
3. 함수객체의 Prototype을 저장
   여기서 prototype은 bind, apply, call 등과 같은 method를 갖고있는 함수자체의 prototype
4. 마지막으로 Environment(스코프), parameter, 함수 몸체, this 참조방식 등의 정보를 저장.

## 함수 생성을 구분하는 이유

Arrow나 method인 경우 생성자로 동작하지 못하도록 방지하고  
함수의 this 참조방식을 결정하기 위해서이다.(Arrow인 경우 Lexical)  
ECMASCript 6 이후 함수를 어떻게 생성하느냐에따라 생성자로 쓰일 수 있는지 없는지 결정.  
함수 생성시 **함수할당 FunctionAllocate**라 부르는 단계에서 결정.  
(단, bable 등 transfiler 사용시 ECMAScript 명세와 다르게 동작함. ex. Arrow에서 constructor를 부를때 babel에서 error가 안날 수 있음.)

## Function

-  일반 객체의 확장
-  생성시점에 역할이 어느정도 결정 (callable, constructor)
-  생성시점에 data를 통해 scope, this 참조방식을 결정

함수 호출 call(F,V,[argumentsList])  
call은 함수객체의 Internal[[call]] method를 수행하는 동작(Abstract operation)

```javascript
F(function : callable이 아니면 error)
V(this)
argumentsList(인자 : 없으면 빈 유사배열)


F.[[call]](V,argumentsList)
```

1. F[[FunctionKind]]가 `classConstructor`라면 Error.
2. callerContext는 현재 실행중인 실행 컨텍스트(Running Execution Context)
3. (PrepareForOrdinaryCall) calleeContext에 새로운 Execution Context를 생성하여 지정.
4. (--assertion : 새로만들어진 calleeContext가 현재 Running Execution Context)
5. (OrdinaryCallBindThis) this를 Binding.
6. (OrdinaryCallEvaluateBody) function code를 수행하고 result에 그 결과를 저장.
7. calleeContext를 Execution Context Stack에서 제거하고 callerContext를 다시 Running Execution Context로 지정.
8. result를 반환.

⬇ 요약

1. 함수를 호출하면 이에 맞춰서 함수를 실행할수 있는 환경을 만들고 초기화.
2. this를 binding.
3. 실제 함수 code를 수행하고 그 결과를 result에 저장.
4. 호출했던 곳으로 복귀.
5. result 반환.

## Ordinary Call

ECMAScript에서 정의하고 있는 내부 동작.

1. PrepareForOrdinaryCall 일반적인 호출에 대비
   Execution Context를 만들고 초기화 시키는 내용을 가진 동작을 추상적으로 표현
   1. 새로운 Execution context를 생성(calleeContext)
   2. calleeContext에 들어갈 lexical Environment를 생성
   3. calleeContext를 Execution Context Stack에 추가
      calleeContext가 Running Execution Context가 됨.
2. OrdinaryCallBindThis 일반적인 호출에 this를 묶음.
   함수객체의[[thisMode]]에 따른 this값 참조를 결정
   Arrow Function, strict mode Environment와 연결됨.
   1. [[ThisMode]]가 lexical인 경우는 다른 처리를 하지않음. -> arrow
   2. [[ThisMode]]가 strict인 경우, 인자로 넘어온 this/arguments를 Environment Recode에 설정.
   3. [[ThisMode]]가 lexical도 아니고 strict도 아닌경우 global의 [[ThisValue]]를 Environment Record
3. OrdinaryCallEvaluateBody 일반적인 호출을 평가함
   1. 변수선언 초기화 Function Declaration Instantiation
   2. 코드 수행 및 결과 반환

## Lexical Environment 어휘 환경

자바스크립트 코드에서 변수나 함수등의 식별자를 정의하는 데 사용하는 객체

```
EC ┌─ Lexical Environment
   └─ Variable Environment
```

Environment Record : 식별자, 참조 or 값 기록
Outer : 또 다른 lexical environment를 참조하는 포인터. 중첩된 자바스크립트 코드에서 스코프 탐색을 하기위해 사용

```javascript
function foo() {
   const a = 1
   const b = 2
   const c = 3
   function bar() {}
}
foo()
```

⬇

```javascript
{
    environmentRecord : {
        a : 1,
        b : 2,
        c : 3,
        bar : <Function>
    },
    outer : foo.[[Environment]]
}
```

위는 단순 함수호출에 하나의 Lexical Environment지만 실제로는 함수, block statement, catch, with ... 등으로 생성, 파괴 되기도함.

## 함수의 Lexical Environment는 언제 만들어 지나

함수의 호출 F[[call]] 3가지 단계

1. PrepareForOrdinaryCall -> Executon Cntext를 새로 만듦. + Lexical Environment 같이 만듦.
2. OrdinaryCallBindThis
3. OrdinaryCallEvaluateBody
   NewFunctionEnvironment 동작?  
   └ return Environment Record{this,super,new.target} + outer를 가진 Environment

## Environment Record - Identifier Bindings

식별자들의 바인딩을 기록하는 객체( 변수, 함수 등이 기록되는 곳. )

```
            Environment Record
          ┌────────┴──────────────────────────┬─────────────────────────┬───────────────────────────────┐
   Declarative Environment Record   Object Environment Record     Global Environment Record       This Binding
          ┌────────┴─────────────────────────┐
*Function Environment Record     Module Environment Record
```

> Function Environment Record <br/> declartive Environment Record에 변수나 함수의 정보가 담겨있다면 function Environment Record는 추가로 NewFunctionEnvironment에서 언급한 `new.target`,`this`,`super`등에대한 정보를 갖는다. ECMAScript5까지는 this Binding을 Execution Context에서 관리했다면 ES6부터는 Environment Record에서 관리. Record가 식별자들의 정보를 관리하는 객체이기 때문에이게 더 맞는 듯.

## Outer - Environment Reference : scope chain

Javascript는 lexical scope를 갖는 언어. 식별자 탐색에 있어서 당연히 scope chain도 포함. outer는 이 scope chain을 위해 존재하는 참조. ECMAScript3까지는 Scope chain이란 용어를 썼지만 ECMAScript5부터는 `Lexical nesting structure`혹은 `Losical nesting of Lexical Environment values`라고 함.

### 함수가 실행되고 변수들을 초기화 할 때

```javascript
const str = 'outerText'
function foo(fn = () => str) {
   const str = 'innerText'
   console.log(fn())
}
foo()
```

함수가 호출될 때, 매개 변수들을 먼저 초기화하고 기본값 매개변수가 있다면 새로운 Environment를 추가로 만들어서 여기에 함수 내부의 변수들을 등록.
이렇게 새 Environment를 만들어 버리기때문에 기본값 매개변수는 함수내부를 참조할 수 없으면서 함수내부에서는 매개변수를 참조할 수 있는 중첩 구조가 가능.

## Lexical Scoping

```javascript
function init() {
   var name = '밖'
   function inner() {
      console.log(name)
      //var name = '안'
      //console.log(name)
   }
   console.log(name)
   inner()
   console.log(name)
}
init()
```

함수를 어디에서 호출하는 지에 따라 범위가 결정되는 것이 아니라 함수를 어디에 선헌했는지에 따라 결정됨.
⟺ 어디서 호출됐는지에 따라 범위가 결정되는 방식 (Dynamic scoping)

---

**Referance**  
[TOAST Meet Up (1),(2),(3)](https://meetup.toast.com/posts/129)
{: .notice--success}
