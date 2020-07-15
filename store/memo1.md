---
title: 'memo1'
toc: true
toc_sticky: true
categories:
   - javascript
last_modified_at: 2020-06-16T08:06:00-05:00
---

### javascript timeing

| language code | ES2020, TypeScript ...| lint & IDE(code 를 작성하는데 도움을 줌) |
| Marchine language<br/>(기계,cpu x, browser, 언어가 실행되는 환경) | Transplier | Complie|
|file |File&deploy||
|laod|browser load| Browser Complie|
|run(runtime)|Borwser parsing<br/>run|run time|
|termivate |Browser close||

변화에 대응하는 여파를 최소화 시켜서 수정할 파일을 적게만드는 것.

## Runtime Execution

파일을 실행하면 메모리에 적재된다.
명령(셋트)과 값(메모리)으로 분리되어 적재됨.
파일에 있는 내용을 메모리에 값과 명령의 세트로 정해져있는 공간에 예뿌게 로딩되어있는 (load라는 절차)

OS : 실행하면 메모리에 적재하자마자 스케줄링세워서 실행해주는 역할만함

instructin : cpu(중앙처리장치) 해석할수있는 명령어 세트

## State Control

메모리의 state(상태) 관리가 어렵다 고정되어있지않고 변하니깡

컴퓨터는 가끔 오작동함

?? 무엇

## Directive Reference

## Blocking

sync Flow가 실행되는 동안 다른일을 할 수 없는 현상

##### Blocking 줄이기

-  sync flow를 짧게 하기
-  다른 threadap syncflow를 떠넘기기
   _ 빨리끝내야하는 thread(main thread : OS가 점유하길 바람) - 늦게끝나도되는 thread
   _ 다른 thread의 작업이 완료되면 원래 thread에 보고 해야 함
   Non Blocking

## Sync & Async

| sync | 서브루틴이 즉시 값을 반환함 |
| Async | 서브루틴이 다른수단으로 값을 반환함 |

Promise
callback function
iterations(Async iterations)

##### Async 단점

우리는 배울때부터 sync로 배워버려따... 8ㅅ8
모든걸 순차적으로 하는 것에 익숙해져있음.

-  호출결과가 즉시 반환되지 않음므로 현재의 sync flow가 종료됨
-  그 결과 현재의 context내의 상태를 결과시점에 사용할 수 없다.
-  요청시의 상태를 별도로 결과시점에 전달할 부가장치 필요

sync의 장점 + async 의장점
sync로직으로 async를 사용할 수 있게 함
하지만 sync flow가 어긋나므로 이전 sync flow의 상태를 기억하여 이어줄 장치 필요
상태를 기억하고 이어주는 장치 continuation
이를 활용하는 프로그래밍 스타일 continuation passing style(CPS에 기반한 프로그래밍)

# Non Blocking JAVASCRIPT

#### Concurrency 동시성(마땅하게 한국말이없옹.. : 마치 동시에 일어나는것 같음)

Parallelism : 병행성 (-> 병렬성)

수행해야할 과업이 2개가있다. 그래야 병렬적으로 처리를 할수가 이따.

## setTimer (동시성을 이해하기위한)

```javascript
const item = class {
   time
   block
   constructor(block, time) {
      this.block = block
      this.time = time + perfomance.now()
      //Date() 그냥 현재 세계시간
      //perfomance : browser가 시작하고나서 몇초가 지났느냐 나노초까지 나옴
   }
}

const queue = new Set()
```

set을 왜써야하느냐

```javascript
const f = time => {
   quere.forEach(item => {
      if (item.time > time) return
      queue.delete(item)
      item.block()
   })
   requestAnimationFrame(f)
}
requestAnimationFrame(f)
```

```javascript
const timeout = (block,time) => queue.add(new Item(block,time))
timeout(_=>console.log('hello',1000)

```

---

# Non Blocking For

```javascript
const working = _ => {}
for (let i = 0; i < 10000; i++) working()
const nonBlockingFor = (max, load, block) => {
   //분산처리
   let i = 0 // 상태를 보존해주고 있음 closer
   const f = time => {
      let curr = load
      while (curr-- && i < max) {
         block()
         i++
      }
      console.log(i)
      if (i < max - 1) timeout(f) //requestAnimationFrame 감춰버림
   }
   timeout(f)
}
```

지연실행 : 함수가 호출되기전까지는 실행되지 않음.

# generator

interface : object의 형태 ; 특정객체에 특정 값이 들어가있는 것

```javascript
const infinity (function*(){
    let i = 0
    while(true) yield i++
})()
console.log(infinity.next())
```

위 코드는 끊임없이 다음 숫자를 넘겨주게되는데 왜 무한루프에 빠져서 블로킹으로 죽지안흔ㄴ거지? (cps의 핵심)

<u>function\*</u>이라는 문법은 내부적으로 서스펜드(suspend : 유보하다, 미결하다, 허공에뜨게하다, 일시정지하다. <->리줌 resume:재개하다 ) 구간을 생성한다.

동기명령을 절대로 멈출수없다? -> 제너레이터는 가능!

메모리에 적재할때 레코드로 감싸서 적재시켜놓음

하나 실행할때 레코드 하나를 푼다.

**yield**가 호출되고나서 서스펜드가 걸린다.

다음번 next()가 호출되어야 돌기 시작한다.

```javascript
const gene = function* (max, load, block) {
   let i = 0,
      curr = load
   while (o < max) {
      if (curr--) {
         block()
         i++
      } else {
         curr = load
         console.log(i)
         yield
      }
   }
}

//새겅
const nonBlockingFor = (max, load, block) => {
   const iterator = gene(max, load, block)
   const f = _ => iterator.next().done || timeout(f)
   timeout(f, 0)
}

nonBlockingFor(100, 10, working)
```

## Promise (반제어권)

callback을 쓰면 제어권을 잃어버린다. callback이 언제 호출되는지 모르기 때문이다.

말이 비동기지 사실은 아무것도 안하고 있다.

우리가 원하는 시점에 callback이 오게 하고 싶다.

내가 원할때 then을 호출할수있다.

```javascript
const gene = function* (max, load, block) {
   let i = 0
   while (i < max) {
      yield new Promise(res => {
         let curr = load
         while (curr-- && i < max) {
            block()
            i++
         }
         console.log(i)
         timeout(res, 0)
      })
   }
}

const nonBlockingFor = (max, load, block) => {
   const iterator = gene(max, load, block)
   const next = ({ value, done }) => done || value.then(v => next(iterator.next()))

   next(iterator.next())
}

nonBlockingFor(100, 10, working)
```

# Continuation Passing Style

## Context & switch

```javascript
// ES6 ver
const gene = function* (a) {
   let b
   yield a
   b = a
   yield b
}
// ES5 ver
;('use strict')
var gene = /*#__PURE__*/ regeneratorRuntime.mark(function gene(a) {
   var b
   return regeneratorRuntime.wrap(function gene$(_context) {
      while (1) {
         switch ((_context.prev = _context.next)) {
            case 0:
               _context.next = 2
               return a

            case 2:
               b = a
               _context.next = 5
               return b

            case 5:
            case 'end':
               return _context.stop()
         }
      }
   }, gene)
})
```

## Continuation & Resume
