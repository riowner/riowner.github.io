---
title: 'generator function'
toc: true
toc_sticky: true
categories:
   - javascript
last_modified_at: 2020-06-16T08:06:00-05:00
---

# Generator Function

`async/await`와 함께 비동기를 동기처럼 ➡ Generator와 기반이 비슷.

## Generator를 왜 써야 할까?

1. 비동기 특성을 동기적 코드방식으로 관리

2. iterator/iterable을 쉽게 사용

3. coroutine(코루틴)
   `function`,`subroutine`,`routine`,`method`,`procedure` ➡ 특정동작을 수행하는 일정 코드 부분.

   ❖ coroutine ? <br/> routine의 일종 → 협동 routine ( co ≒ with, together ) <br/> 상호 연계 프로그램 <br/> routine과 subroutine은 서로 비대칭적인 관계.<br/> `coroutine`들은 완전 대칭 ➡ 서로가 서로를 호출하는 관계  
    ↳ suspend/resume 가능 ! 함수는 suspend와 resume이 빠진 coroutine.

   ❖ coroutine의 특성을 subroutine과 비교

   -  일반 **function(subroutine)**`(실행되는 시작점이 언제나 같다.)`
      caller가 함수를 call하면 callee함수는 call stack에 들어와 작동.
      자신의 로직을 수행완료하면 값을 returngkrh call stack에서 사라짐.
      call stack에서 나가면 붙잡을수도 실행할 수 도 없다.
      만약 다시 실행 된다고 해도 처음부터 다시 시작해야 한다.
   -  **coroutine**  
       종료된 함수라고 해도 다시 실행될 때 그 진입점을 자신이 원하는 곳으로 커스터 마이징 가능.
      generator(coroutine 특성) generator 객체가 함수의 stack frame(각종 context)를 저장하고 있음.
      이 객체가 **yield**를 만나면 함수가 잠시 suspend되는 동시에 함수 context를 복사하고 call stack을 벗어난다.
      caller가 next() method를 call하면 저장해둔 stack frame들이 복원됨.
      잠시 suspend되었던 함수는 `복원된 context`에서 실행.

   -  코루틴은 우리가 잘 알고 있는 서브루틴(Subroutine)과 달리 진입점(Entry Point)이 여러 개일 수 있습니다.
      쉽게 이야기하면 실행을 멈췄다가(Suspend) 재개(Resume)할 수 있다는 점인데요.
      이 특성을 살리면 우리가 익히 아는 스레드(Thread)처럼 쓸 수 있게 됩니다.
      다만 스레드와 달리 코루틴은 비선점적(Non-Preemptive)이기때문에
      코드의 흐름을 전적으로 사용자가 제어할 수 있습니다

4. 동시성  
   coroutine을 잘 이용하면 협력형 멀티 태스킹 방식으로 thread`(필요한 비용에 비해 제어가 많음.)` 프로그래밍 없이 동시성 프로그래밍 가능.
   coroutine : OS의 암묵정인 scheduling/context/switching over head/Semaphore 설정 등의 고민으로부터 자유롭다.

5. 비동기  
   동기적으로 작동되는 coroutine 특성
   code가 작성된 형태는 **동기적** → 실행은 **비동기**

6. 메모리 효율  
   Lazy-Evaluation`(느긋한 계산 : 값을 받기까지 그 시간을 지연시키고 늦추는 것. 필요할 때만 값을 요구.)`을 통해 필요할 때 값을 요구해서 메모리를 효율적으로 사용.

## Generator 어디에 사용할까?

1. Promise (Async/await)  
   Promise로도 비동기 code를 다루는데에 문제가 없지만 generator와 함께 사용하면 code의 가독성과 관리적 측면 ⬆  
   generator 함수 형태가 Async/Await`async함수는 promise를 반환하며 callback함수가 성공/실패에 관계없이 await에서 값을 받을 때까지 대기`와 비슷

```javascript
function* generator_promise() {
   let a = yield Promise1()
   console.log(a)
   let b = yield Promise1()
   console.log(b)
   let c = yield Promise1()
   console.log(c)
}
```

generator_promise는 매번 실행하여 yield를 만나 정지함.

2. Infinite Data Generator  
   generator는 기본적으로 값을 생성(generate)하는 무언가..
   값이 생성되는 조건이 무한일 경우 Infinite Data Generate할 수 있음.
   함수의 생성자와는 다르다.
3. Lazy-Evaluation  
   계산의 결과값이 필요한 시점까지 계산을 늦춤.
   연산 후 일정시간을 지연 시켜 값을 필요한 시점에 사용가능.
   필요없는 계산을 하지 않으므로 실행 빨라지고, 복함수식 계산시 오류상태를 피함.
   무한 자료구조, 미리 정의된 것을 이용하지 않고, 보통 함수로 제어구조 정의.

## Generator 함수란 무엇인가

1. 정의
   generator함수가 반환하는 객체는 곧 객체 자신이며, generator를 실행하는 것은 iterator를 실행하는 것과 같다.

2. 일반함수와 generator함수의 차이점

   -  Run-to-Completion`모두실행`과 coroutine`suspend/resume`
      javascript의 특징 Run-to-Completion
      한번 실행한 task는 무조건 종료되어야 다른 task가 실행됨.
      중간에 interrupt/termination은 없다.
   -  `일반함수는`value`를 return` & `generator는` iterator`를 return`
   -  값을 유지한다.
      일반함수는 함수 자체가 종료되고 call stack을 빠져나오면서
      function의 Body에 설정한 값이 초기화 됨.(저장 X, closer 이용)
      generator는 수행하는 로직에 값이 계속 유지가 됨.

## yield

함수를 중지하거나 재개하는데 사용.

1. Generator Function에서 주도권? caller`함수를 호출 하는 대상` VS callee`호출된 대상`

```javascript
function* generate() {
   // callee
   let a = yield 20
}

const generator = generate()
// caller : next() method 호출을 통해 yield 이후를 제거 가능
generator.next(10)
```

2. next() method의 활용의미와 인자 여부

```javascript
next(_________) //yield 왼쪽 변수의 값으로 할당
```

-  next() method 데이터를 요청
   next()를 호출하는 목적은 iterable한 객에츼 값을 하나씩 출력
-  next() 인자 여부 - 활용성
   -  없는 경우 : caller가 주도권을 가짐 server데이터 요청 DB 값 좀 줘.
   -  있는 경우 : 어떤 값을 전달함. server에서 (인자에 넣은) 어떤 값을 줘.


