---
title: \[javascript] 프로토타입
categories:
- Javascript
toc: true
toc_sticky: true
published : false
---

> 코어 자바스크립트 책에 있는 내용과 예제를 요약한 포스트입니다.

클래스 기반 언어 : 상속을 사용함.

프로토타입 기반 언어 : 객체를 원형(prototype)으로 삼고 참조함으로써 상속과 비슷한 효과를 얻음. 

# 1. 프로토 타입의 개념 이해

## 1.1. construtor, prototype, instance

```javascript
var instance = new Contructor();
```

![prototype]({{ site.url }}{{ site.baseurl }}/assets/images/javascript/prototype.png)

- 생성자 함수 `Contructor`를 `new`와 함께 호출
- `Constructor`에서 정의된 내용을 바탕으로 새로운 `instance`가 생성
- 이 때 instance에는 `__proto__` 프로퍼티가 자동으로 부여되는데
- `__proto__`는 `Contructor`의 `prototype` 이라는 프로퍼티를 참조

`prototype` 객체 내부에는 인스턴스가 사용할 메서드를 저장하며,
인스턴스에서 이를 참조하는 `__proto__` 를 통해 이 메서드에 대해 접근할 수 있게 된다.


**[참고]** 실제로 `__proto__`에 접근할 때는, `Object.getPrototypeOf() / Object.create()`등을 이용한다.


```javascript
var Person = function(name){
    this._name = name;
};
Person.prototype.getName = function(){
    return this._name;
};
var suzi = new Person('Suzi');
suzi.__proto__.getName();   // undefined
Person.prototype === suzi.__proto__ // true
```


```javascript

```
```javascript

```
```javascript

```