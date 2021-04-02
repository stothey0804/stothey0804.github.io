---
title: \[javascript] 실행 컨텍스트
categories:
- Javascript
toc: true
toc_sticky: true
---


> 코어 자바스크립트 책에 있는 내용과 예제를 요약한 포스트입니다.

실행 컨텍스트(execution context)는 실행할 코드에 제공할 환경 정보를 모아놓은 객체로, 
자바스크립트의 동적 언어로서 성격을 가장 잘 파악할 수 있는 개념이다.

자바스크립트는 어떤 실행 컨텍스트가 활성화 되는 시점에 
hoisting, 외부 환경 정보 구성, this 값 설정 등의 동작을 수행한다.

- - - 

# 1. 실행 컨텍스트란?

실행할 코드에 제공할 환경 정보들을 모아놓은 객체. 이를 call stack에 쌓아올렸다가, 
가장 위에 쌓여있는 컨텍스트와 관련있는 코드들을 실행하는 식으로 전체 코드의 환경과 순서를 보장한다.

하나의 실행 컨텍스트를 구성하는 방법으로 전역공간, eval()함수, 함수가 있으며
자동으로 생성되는 것을 제외하면 실행 컨텍스트를 구성하는 방법은 함수를 실행하는 것 이다.

```javascript
// ---------- (1) 전역 컨텍스트
var a = 1;
function outer(){
    function inner(){
        console.log(a); // undefined
        var a = 3;
    }
    inner(); // ------ (2) 
    console.log(a); // 1
}
outer();  // ------ (3) 
console.log(a); // 1
```

위 예제에서 코드를 실행하는 순간 (1) 전역 컨텍스트가 콜스택에 담기고, 
(3) 에서 outer를 호출하면 outer 실행 컨텍스트를 생성하고 콜스택에 담는다.

콜스택에서 (3) outer 실행 컨텍스트가 놓은 상태가 되어 outer 함수 내부의 코드들을 순차로 실행한다.

다음 (2) inner  함수의 실행 컨텍스트가 콜 스택에 가장 위에 담기고 내부 코드를 순차로 실행한다.

실행 컨텍스트가 활성화 될 때, 자바 스크립트 엔진은 관련 코드를 실행하는데 필요한 환경정보를 수집해서 실행 컨텍스트 객체에 저장한다. (엔진이 활용할 목적으로 생성하므로 개발자가 확인할 수 없음)

수집 정보는 다음과 같다.

- `VariableEnvironment` : 현재 컨텍스트 내의 식별자들에 대한 정보 + 외부 환경정보, 선언 시점의 `LexicalEnvironment`의 snapshot. 변경 사항은 반영되지 않음.
- `LexicalEnviroment` : 초기에는 `VariableEnvironment`와 같지만 변경 사항이 실시간으로 반영됨. 
- `ThisBinding` : `this` 식별자가 바라봐야할 대상 객체


# 2. VariableEnvironment

`VariableEnvironment` 에 담기는 내용은 `LexicalEnvironment` 와 같으나, 최초 실행 시의 스냅샷을 유지한다는 점에 차이가 있다. 실행 컨텍스트를 생성할때 `VariableEnvironment` 에 정보를 담은 다음, 이를 복사한 `LexicalEnviroment`를 주로 활용한다.

내부는 `environmentRecord`와 `outer-EnvironmentReference` 로 구성되어있다.


# 3. LexicalEnvironment

## 3.1. environmentRecord와 호이스팅

`environmentRecord`에는 현재 컨텍스트와 식별자 정보가 저장된다. **함수에 지정된 매개변수 식별자, 함수 그 자체, `var`로 선언된 변수의 식별자** 등. 컨텍스트 내부 *순서대로* 수집한다.

**[참고]** 전역 실행 컨텍스트는 변수 객체를 생성하지 않고 전역 객체(global object: window, global)를 활용한다. 이는 자바스크립트 내장 객체(native object)가 아닌 호스트 객체(host object)로 분류된다. {: .notice}

따라서 자바스크립트 엔진은 코드가 실행되기 전에도 해당 환경에 속한 코드의 변수명을 알고있는 상태가 되며,
이를 호이스팅(hoisting)이라는 개념으로 간주한다.

### 호이스팅 규칙

다음은 호이스팅 규칙을 위해서 정리된 예제다. (실제로 동작하는 것과 관계 없음.)

```javascript
function a(x){              // 수집대상 1(매개변수)
    console.log(x); // (1) 
    var x;                  // 수집대상 2(변수 선언)
    console.log(x); // (2) 
    var x = 2;              // 수집대상 3(변수 선언)
    console.log(x); // (3) 
}
a(1);
```

위 코드에서 호이스팅이 되지 않는다고 가정하면
(1) 1, (2) undefined, (3) 2 가 출력될 것으로 예상된다.

- 매개변수를 선언 / 할당과 같다고 간주

```javascript
function a(){
    var x = 1;          // 1
    console.log(x);
    var x;              // 2 
    console.log(x);
    var x = 2;          // 3
    console.log(x);
}
a();
```

- 매개 변수 수집

```javascript
function a(){
    var x;
    var x;
    var x;

    x = 1;
    console.log(x); // (1) 1
    console.log(x); // (2) 1
    x = 2;
    console.log(x); // (3) 2
}
```

호이스팅을 마쳤을 경우 예상한 것과 달리 위와 같은 결과가 나온다.
다음 코드도 호이스팅을 이해하기 위해 정리된 예제이니 참고해보자. 

```javascript
function a(){
    console.log(b); // (1)
    var b = 'bbb';
    console.log(b); // (2)
    function b(){}
    console.log(b); // (3)
}
a();
```
호이스팅이 안된다고 가정하면 
(1) undefined (2) 'bbb' (3) function
로 예상된다.

```javascript
function a(){
    var b;
    function b(){}

    console.log(b); // (1) function
    b = 'bbb';
    console.log(b); // (2) 'bbb'
    console.log(b); // (3) 'bbb'
}
a();
```

호이스팅이 적용되면 위와 같은 결과가 나온다.


### 함수 선언문과 함수 표현식

- 함수 선언문(function declation) : 함수의 정의부만 존재하는 것. 함수명이 반드시 정의되어야함.

- 함수 표현식(function expression) : 정의한 함수를 별도 변수에 할당한 것. 일반적으로 익명함수 표현식을 말함.

```javascript
function a() { /* */ }          // 함수 선언문.
a();    // 실행

var b = function() { /* */ }    // 익명 함수 표현식
b();    // 실행

var c = function d() { /* */ }  // 기명 함수 표현식. 변수명 c, 함수명 d
c();    // 실행
d();    // 에러 
```

마지막 라인에서 기명 함수 표현식은 외부에서 함수명으로 호출 불가하기 때문에 에러가 난다.
예전에는 디버깅을 위해 사용했으나 현재는 모든 브라우저가 익명 함수 표현식을 추적할 수 있으므로
의미가 없을 듯함.

- - -

- 함수 선언문과 표현식의 차이

```javascript
console.log(sum(1, 2));
console.log(multiply(3, 4));    // multiply 에 값이 할당되어있지 않음.

function sum(a, b){
    return a + b;
}
var multiply = function(a, b){
    return a * b;
}
```

함수 선언문으로 정의된 sum 함수는 선언 전에 호출해도 문제 없이 실행되며, 함수 표현식으로 정의된 multiply의 경우 선언 전에 호출할 경우 오류가 발생한다.

함수 선언문을 사용할 경우, 같은 파일 내에 같은 함수명을 가진 함수를 정의하게 된다면
나중에 선언한 함수선언문으로 재정의된다. 


## 3.2 스코프, 스코프 체인, outerEnvironmentReference

scope란 식별자에 대한 유효범위이다. ES5까지의 자바스크립트는 전역공간을 제외하면 함수에 의해서만 스코프가 생성되며, 이 유효범위를 안에서 부터 바깥으로 검색하는 것을 스코프 체인(scope chain)이라 한다. 

이 스코프 체인을 가능하게 하는 것이 `LexicalEnvironment`의 두 번째 수집 자료인 `outerEnvironmentReference`다.

### 스코프 체인

`outerEnvironmentReference`는 호출된 함수가 **선언될 당시**의 `LexicalEnvironment`를 참조한다.  

스코프 체인상 가장 먼저 발견된 식별자에만 접근이 가능하다.

```javascript
var a = 1;
var outer = function(){
    var inner = function(){
        console.log(a); // undefined (variable shadowing)
        var a = 3;
    };
    inner();
    console.log(a); // 1 (outer -> global 탐색)
};
outer();
console.log(a); // 1 (global 탐색)
```

### 전역변수와 지역변수

- 전역변수(global variable)는 전역 공간에서 선언한 변수
- 지역변수(local variable)는 함수 내부에서 선언한 변수

코드의 안정성을 위해 가급적 전역변수 사용을 최소화하는 것이 좋다.

## 정리

- 실행 컨텍스트는 실행할 코드에 제공할 환경 정보를 모아 놓은 객체로, 활성화 시점에 `VariableEnvironment`, `LexicalEnvironment`, `ThisBinding`의 세 가지 정보를 수집한다.

- 실행 컨텍스트를 생성할 때는 `VariableEnvironment`, `LexicalEnvironment` 가 동일한 내용으로 구성되는데, `LexicalEnvironment`는 함수 실행 도중 변경되는 사항이 반영되며, `VariableEnvironment`는 초기 상태를 유지한다.

- `VariableEnvironment`, `LexicalEnvironment`는 선언한 함수의 함수명 등을 수집하는 `environmentRecord`와 직전 컨텍스트의 `LexicalEnvironment` 정보를 참조하는 `outerEnvironmentReference`로 구성되어 있다.

- 호이스팅(hoisting)은 `environmentRecord`의 수집과정을 추상화한 개념으로, 실행 컨텍스트가 관여하는 코드 집단의 최상단으로 끌어올린다고 해석하는 것이다. 여기서 함수 선언문과 함수 표현식의 차이가 발생한다.

- 스코프(scope)는 변수의 유효범위를 말하며, 코드 상에서 변수에 접근하려고 하면 현재 컨텍스트의 `LexicalEnvironment`를 탐색하고, 발견하지 못하면 `outerEnvironmentReference` 를 통해 전역까지 탐색한다. 

- 전역 컨텍스트의 `LexicalEnvironment`에 담긴 변수를 전역변수라 하고, 그 외 함수에서 생성된 변수는 지역변수이며, 안전한 코드 구성을 위해 가급적 전역 변수의 사용은 최소화하는 것을 권장한다.

