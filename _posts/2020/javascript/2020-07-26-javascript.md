---
title: JavaScript - 변수, 연산자, 타입
Categories: Javascript
---

JavaScript(JS)는 객체 기반의 스크립트 프로그래밍 언어다.
스크립트 언어는 다른 응용프로그램에서 동작하는 언어로, 컴파일하지 않고도 실행할 수 있다.

자바스크립트는 웹 브라우저 내에서 주로 사용되며, 
ECMAScript(ES)의 버전에 따라서 자바스크립트 실행 엔진이 반영하는데, 2018년 기점으로  ES6(ES2015)을 지원하는 브라우저가 많아서 ES6 문법이 표준으로 쓰이고 있다. 


<hr>

### 1. 변수의 선언

* **var**
	function 내 : 함수 내 지역변수 범위 / function 밖 : 전역변수의 범위
	
* **let**
블록 범위

* **const**
선언 위치에 따라 범위가 다름. 
재할당이 불가능하므로 선언시 초기화해야한다.

변수에 따라 scope, 변수의 유효범위가 달라진다.
자바스크립트의 중요한 특징이다.

```
var x = 0;  // x는 전역으로 선언되었고, 0으로 할당.
console.log(typeof z); // undefined
function a() { // a 함수를 호출했을 때
  var y = 2;   // 함수 a에서 지역변수로 선언되었으며, 2로 할당.
console.log(x, y);   // 0 2 
function b() {       // b 함수를 호출하였을때,
    x = 3;  // 존재하는 전역 x값에 3을 할당, 
    y = 4;  // 존재하는 외부 y값에 4를 할당, 
    z = 5;  // 새로운 전역 z 변수를 생성하고 5를 할당 합니다. 
  }         // (strict mode에서는 ReferenceError를 출력합니다.)
b();     // 호출되는 b는 전역 변수로 z를 생성합니다.
  console.log(x, y, z);  // 3 4 5
}
a();                   // 호출되는 a는 b를 호출합니다.
console.log(x, z);     // 3 5
console.log(typeof y); // undefined y는 function a의 지역 변수.
```

> <font size="2">출처: https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/var</font>



```
function varTest() {
  var x = 1;
  if (true) {
    var x = 2;  // 상위 블록과 같은 변수!
    console.log(x);  // 2
  }
  console.log(x);  // 2
}
function letTest() {
  let x = 1;
  if (true) {
    let x = 2;  // 상위 블록과 다른 변수
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```
> <font size="2">출처: https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let</font>

<br>

### 2. 연산자

연산자는 기존 프로그래밍 언어와 유사하게 지원하는데, 참고할 부분만 간추렸다.

##### (1) || (or) 연산자
값이 없으면 오른쪽 값을 할당한다.

```var name = "sykim";
var result = name || "defaultname";
console.log(result) // sykim
var name = "";
var result = name || "defaultname"
console.log(result) // defaultname
```
##### (2) 삼항연산자 
```
const data = 11;
const result = (data>10)? "ok": "fail";
document.write(result);
```
##### (3) 비교연산자

자바스크립트는 ==로 비교하면, 
암묵적으로 타입을 바꾸어 비교하므로 =(equal)을 3개 넣어서 비교한다.
```
0 == false // true
"" == false // true
null == false // false
0 == "0" // true
null == undefined // true


0 === "0" // false
```
<br>

### 3. Type의 종류

```undefined, null, boolean, number, string, object, function, araay, Date, RegExp```

자바스크립트는 실행할 때 타입이 결정되는 언어로, 느슨한 언어(loosely typed language)라고 불린다.
그래서 다른 언어와 다르게 변수에 포함할 데이터 타입을 지정하지 않아도 된다.

typeof 또는 Object.prototype.toString.call() 로 타입을 확인할 수 있는데,
toString을 사용하면 어떤 객체의 인스턴스인지 정확히 알려준다.



>MDN 문서
> <font size="2">https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference</font>

>참고 강의
> <font size="2">https://www.edwith.org/boostcourse-web/lecture/16693/</font>
