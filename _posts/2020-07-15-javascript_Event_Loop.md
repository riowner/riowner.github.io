---
title: 'javascript Event Loop'
toc: true
toc_sticky: true
categories:
   - javascript
last_modified_at: 2020-06-16T08:06:00-05:00
---

javascript는 ProtoType 기반 언어 / 동적 언어

```
┌─Stack──────────────┐┌─Heap──────────────────────────────┐
│ 코드가 실행되며        ││ 동적으로 생성된 인스턴스가 할당           │
│ stack frame이       ││ (구조화 되지않은 더미같은 메모리 영역)    │
│ 쌓이는 곳            ││                                   │
│                    ││                                   │
└────────────────────┘└───────────────────────────────────┘
┌─(Task, Event)Queue──────────────────────────────────────┐
│ Task 들을 임시 저장하는 대기 큐                               │
│ Call Stack이 비어졌을때 먼저 대기열에 들어온 순서대로 수행됨.       │
│ micro task 환경에서 순차적으로 실행되어야 하는 작업.              │
│ ex. setTimeOut, callBack                                │
│                                                         │
└─────────────────────────────────────────────────────────┘
┌─Micro Task──────────────────────────────────────────────┐
│ 일반 task보다 높은 우선순위를 가짐.                            │
│ ex. Promise, Observer API, Node.js-process.nextTick     │
└─────────────────────────────────────────────────────────┘
```

```javascript
setTimeout(() => {
   console.log('set time out')
})
Promise.resolve().then(() => {
   console.log('promise')
})
requestAnimationFrame(() => {
   console.log('rea')
})
```
