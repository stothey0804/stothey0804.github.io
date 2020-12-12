---
title: javascript - 함수
categories:
- Javascript
toc: true
---

## 함수 (Function)

자바스크립트의 핵심은 function이므로 잘 이해 해야할 것이다!

**자바스크립트 합수의 특징**

1. 파라미터의 개수를 검사하지 않는다. (일치하지 않아도 실행된다.)
2. 파라미터의 type을 검사하지 않는다.


### 함수 선언, 함수 표현식

**함수의 선언**

```js
function myFunction(param) {
    var name = "myName";
    return name + " : " +param;
}
```

**함수 표현식**

```js
var printName = function() {
    return 'anonymouse';
}
```

함수표현식으로 선언할 경우 함수의 이름을 생략할 수 있다.
이 경우 함수선언문과 달리 호출순서에 따라 함수가 실행되지 않을 수 있다.
(호이스팅 항목 참조)


**IIFE (Immediately Invoked Function Expression)**

```js
(function () {
    statements
})();

```

즉시 실행되는 함수 표현식, 자체실행 익명함수라고도 한다.
표현식 내의 변수는 외부에서 액세스할 수 없다. IIFE를 변수에 할당하면 함수의 반환값이 저장된다.

```js
(function () {
    var aName = "Barry";
})();
// Variable aName is not accessible from the outside scope
aName // throws "Uncaught ReferenceError: aName is not defined"

```

```js
var result = (function () {
    var name = "Barry";
    return name;
})();
// Immediately creates the output:
result; // "Barry"

```

> MDN - IIFE
> : <https://developer.mozilla.org/en-US/docs/Glossary/IIFE>




**함수의 리턴값**

자바스크립트 함수는 반드시 return값이 존재하며,
없을 경우는 기본 반환값인 'undefined'가 반환된다.

__[참고]__ 자바스크립트에는 void 타입이 없다.
{:.notice--info}

### 호이스팅 (Hoisting)

자바스크립트 함수는 실행되기 전 자바스크립트 파서에 의해 코드가 끌어올려지는데,
이를 호이스팅이라고 한다. 따라서, 함수의 선언부 이전에 해당 함수를 사용하는게 가능하다.

[예제]

```js
catName("Chloe");

function catName(name) {
  console.log("My cat's name is " + name);
}
/*
위 코드의 결과는: "My cat's name is Chloe"
*/
```

단, 초기화부분이 아닌 선언만 끌어올리기 때문에 함수표현식은 끌어올려지지 않는다.
{:.notice}

>MDN - Hoisting
> : <https://developer.mozilla.org/ko/docs/Glossary/Hoisting>


- - -

### argument 객체

함수가 실행되면 arguments라는 객체가 지역변수로 자동 생성되며,
함수에 선언한 파라미터보다 많은 파마미터를 받았을 때, arguments 객체를 배열 형태로 접근할 수 있다.

단, 배열이 아니기 때문에 Array 메서드를 사용할 수 없다.
{:.notice}

[ex]
```js
function func(){
  var result = 0;
  for(var i in arguments){
    result += arguments[i];
  }
  return result;
}
func(1,2,3,4,5,6,7,8,9,10); // 55
```

### 콜백함수

파라미터로 함수를 받으며, 보통 나중에 실행되는 함수를 callback 함수라고 한다.

[ex]
```js
function calc(num1, num2, operator){
    console.log(operator(num1, num2));
}

function add(num1, num2){ return num1 + num2; }
function sub(num1, num2){ return num1 - num2; }
function multi(num1, num2){ return num1 * num2; }
function div(num1, num2) {return num1 / num2; }

calc(4,2,add); // 6
calc(5,2,sub); // 3
calc(4,2,multi); // 8
calc(4,2,div); // 2
```



### 클로저

외부함수가 감싸고 있는 내부함수가 외부함수의 context에 접근할 수 있는 것을 말한다.

```js
function outter(){  // 외부함수
    var title = 'coding everybody';
    function inner(){ // 내부함수
        alert(title);
    }
    inner();
}
outter(); // coding everybody 출력
```

#### 캡슐화

자바스크립트는 private 접근자를 지원하지 않지만, 아래 예제처럼 클로저를 이용하면 구현이 가능하다.
또, 외부함수의 지역변수를 사용하는 내부함수가 소멸될때까지 사라지지 않는 특성이 있다.


```js
var counter = (function() {
var privateCounter = 0;
function changeBy(val) {
    privateCounter += val;
    }
    return {
        increment: function() {
            changeBy(1);
        },
        decrement: function() {
            changeBy(-1);
        },
        value: function() {
            return privateCounter;
        }
    };   
})();

console.log(counter.value()); // logs 0
counter.increment();
counter.increment();
console.log(counter.value()); // logs 2
counter.decrement();
console.log(counter.value()); // logs 1
```



> 참고 - 생활코딩 Javascript
> : <https://opentutorials.org/course/743/6544>

> MDN - 클로저
> : <https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Closures>
