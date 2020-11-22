---
title: \[javascript] 호출 스택(Call Stack)
categories:
- Javascript
---

## 호출 스택(Call Stack) 이란?

자바스크립트 엔진은 Memory Heap과 Call Stack(호출 스택)으로 으로 구성되어있다.
자바스크립트는 일반적으로 단일 스레드이며, 이는 Call Stack도 단일이라는 뜻이다.
(자바스크립트 = 단일스레드는 100% 맞는 말은 아닌데 이 부분은 좀 더 공부하면서 정리하려고 한다...)

<img src="https://miro.medium.com/max/2048/1*4lHHyfEhVB0LnQ3HlhSs8g.png">

> 출처 🔗<a href="https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf">How JavaScript works: an overview of the engine, the runtime, and the call stack</a>


- Memory Heap : 메모리 할당이 일어나는 영역. 변수, 함수 등
- Call Stack : 코드가 실행 될 때, Stack형태로 쌓이는 영역.

### 예제

```javascript
function first(){
    second();
    console.log('1');
}
function second(){
    third
    console.log('2');
}
function third(){
    console.log('3');
}
first();
```

> 실행결과 :
> 3
> 2
> 1

<img src="/assets/images/javascript/callback.png">


함수 호출 부분을 만나면 호출한 함수를 호출 스택에 넣고, 역순으로 실행합니다 (Stack의 후입선출 구조)


```javascript
function run(){
    console.log('3초 후 실행');
}
console.log('시작');
setTimeout(run,3000);
console.log('끝');
```

> 실행결과 :
> 시작
> 끝
> 3초 후 실행

`setTimeout()`의 경우 특정 시간 이후 코드를 실행하므로 run()은 나중에 실행된다.


> 참고사이트 https://developer.mozilla.org/ko/docs/Glossary/Call_stack