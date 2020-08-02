---
title: javascript - 형변환, String 메서드, Array 메서드
categories:
- javascript
toc: true
---

## javascript 형(type)

javascript는 실행될 때 타입이 결정되는 언어이므로, 정확한 연산이 필요할 때 원하는 타입으로 객체를 초기화하거나, 형변환을 해야한다.


### literal
리터럴(literal)이란 소스 코드의 고정된 값을 대표하는 용어로, Javascript에서 값을 나타내기 위해 리터럴을 사용한다.

* 정수
  * 10진수, 16진수, 8진수, 2진수 표현

* 부동소수점
  ```
  [(+|-)][digits][.digits][(E|e)[(+|-)]digits]
  ```

* boolean
  ```
  ture | false
  ```

* String
  * '또는 "로 묶인 0개이상의 문자
  [ex]
  ```js
  "foo"
  'bar'
  "one line \n another line"
  ```
  * 문자열 리터럴 값은 문자 객체의 모든 메서드를 호출 할 수 있다. // "foo".length <- 3

* 배열
  * var myList = []; // 배열로 초기화
  * var myList = ['one', , 'three'] // 지정되지 않은 요소는 undefined
  * 빠진 요소의 값은 명시적으로 undefined 선언하는 것이 명확성과 유지보수 측면에서 좋다.

* 객체
  * {}로 묶인 0개 이상의 객체 속성-값 쌍 목록.
  ex
  ```js
  var car = { manyCars: {a: "Saab", "b": "Jeep"}, 7: "Mazda" };
  console.log(car.manyCars.b); // Jeep
  console.log(car[7]); // Mazda
  ```

> 위키백과 - 리터럴
> : <https://ko.wikipedia.org/wiki/%EB%A6%AC%ED%84%B0%EB%9F%B4>

> MDN Javascript  Values, variables and literals
> : <https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Values,_variables,_and_literals>


### 형 변환

타입이 다른 변수끼리 사칙 연산 수행 시
+ 는 문자열 우선, 그 외는 숫자를 우선하는 점을 참고한다.


#### 숫자 -> String
* 1 _+ ""_;
* String(1);
* 1.toString();
* (1.123).toFixed(0); // 부동소수점 숫자, 0 -자리수 제한

#### String -> 숫자

* parseInt("1");
* parseFloat("3.14");
* "1" _* 1_

- - -

## javascript String

* 선언
```js
var x = "John Doe";
var x = 'John Doe';
```

### methods

* length : 배열의 길이 리턴 (메서드는 아니고 프로퍼티이다.)

```js
var txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
var sln = txt.length; // 26
```

* indexOf() : 해당 문자열을 *첫번째로* 만나는 위치를 리턴

```js
var str = "Please locate where 'locate' occurs!";
var pos = str.indexOf("locate"); //7
var pos2 = str.indexOf("locate", 15); // 21

```

* lastIndexOf() : *뒤에서 부터* 탐색하여 처음 만나는 위치 리턴

```js
var str = "Please locate where 'locate' occurs!";
var pos = str.lastIndexOf("locate"); //21
```

* search() : *문자열* 을 검색

```js
var str = "Please locate where 'locate' occurs!";
var pos = str.search("locate"); // 7
```



{% capture notice-2 %}
**[참고]** indexOf()와 search()의 차이
1. search()는 시작 위치를 지정하는 인자를 받지 않는다.
2. indexOf()는 정규식을 취할 수 없다.
{% endcapture %}



* slice(start, end) : start ~ end-1 까지 추출. 음수일 경우, string의 끝부터 추출한다.

```js
var str = "Apple, Banana, Kiwi";
var res = str.slice(7, 13); // Banana
```

```js
var str = "Apple, Banana, Kiwi";
var res = str.slice(-12, -6); // Banana
```

* substring(start, end) : slice와 동일하나 음수를 사용할 수 없다.

```js
var str = "Apple, Banana, Kiwi";
var res = str.substring(7, 13); // Banana

```

* substr(start, length) : start 부터 해당하는 길이만큼 추출. (범위 음수 가능)

```js
var str = "Apple, Banana, Kiwi";
var res = str.substr(7, 6); // Banana
var res2 = str.substr(-4); // Kiwi
```

* replace() : 해당 문자열을 대체

```js
str = "Please visit Microsoft!";
var n = str.replace("Microsoft", "W3Schools");
/* Please visit W3Schools! */
```

* toUpperCase(), toLowerCase() : 대소문자 변경


* concat() : 첫번째 인자로 받은 구분자로 String을 연결

```js
var text1 = "Hello";
var text2 = "World";
var text3 = text1.concat(" ", text2); // Hello world!
```

**[참고]** concat() 메서드가 아닌 + 연산자로 string을 연결했을 때는 원본 string을 수정하는게 아니고, 대체하는 것이다.
{: .notice}



* trim() : 좌우 공백제거

```js
var str = "       Hello World!        ";
alert(str.trim()); // Hello World!
```

* split() : 인자로 설정한 구분자를 기준으로 String을 배열로 분리해준다.

```js
var txt = "a,b,c,d,e";   // String
txt.split(",");          // Split on commas
txt.split(" ");          // Split on spaces
txt.split("|");          // Split on pipe

```

> w3schools - JavaScript String Methods
> : <https://www.w3schools.com/js/js_string_methods.asp>


## javasciprt Array

* 선언

```js
var cars = ["Saab", "Volvo", "BMW"];
var cars = new Array("Saab", "Volvo", "BMW");
```

### methods

* toString() : array를 ,로 구분된 string으로 변환한다.

* join(" ") : toString과 유사하며 구분자를 지정한다.

* pop() : 마지막 요소를 리턴 후 제거

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.push("Kiwi");       //  Adds a new element ("Kiwi") to fruits
```

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var x = fruits.pop();      // the value of x is "Mango"
```

* push() : array의 마지막 부분에 요소 추가 후 배열의 길이 리턴.

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.push("Kiwi");       //  Adds a new element ("Kiwi") to fruits
```

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var x = fruits.push("Kiwi");   //  the value of x is 5
```

* shift() : 맨 앞의 요소를 리턴 후 삭제

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.shift();            // Removes the first element "Banana" from fruits
```

* unshift() : 맨앞에 요소를 추가하고, 배열의 길이를 리턴

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.unshift("Lemon");    // Adds a new element "Lemon" to fruits
```

* delete : 해당 인덱스의 요소 삭제

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
delete fruits[0];           // Changes the first element in fruits to undefined
```


* splice(): 해당 위치부터 n개의 요소를 삭제 (하고 요소를 추가할 수 있다.)

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(2, 0, "Lemon", "Kiwi"); // 2번 위치에서 삭제하지 않고 추가
```

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.splice(0, 1);        // Removes the first element of fruits
```


* concat() : 배열을 연결하여 리턴

```js
var myGirls = ["Cecilie", "Lone"];
var myBoys = ["Emil", "Tobias", "Linus"];
var myChildren = myGirls.concat(myBoys);   // Concatenates (joins) myGirls and myBoys
```

concat()은 배열을 변경하지 않고 항상 새로운 배열을 리턴한다.
{: .notice}


* slice(start, end) : 해당위치의 요소를 포함한 배열을 리턴 (원본을 지우지 않는다.) (end 생략 가능, end-1)

```js
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(1);
```

```js
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(1,3); // Orange, Lemon
```

* sort() : 요소를 오름차순으로 정렬한다.

* reverse() : 요소를 내림차순으로 정렬한다.


**[주의]** sort()는 문자기준으로 정렬한다. 숫자 정렬을 하고 싶을 경우 비교 기준 function을 주어야하는 점에 주의할 것
{: .notice--warning}

```js
var points = [40, 100, 1, 5, 25, 10];
points.sort(function(a, b){return a - b});

```

### iteration methods

* forEach() : 배열의 각 요소에 따라 파라미터에 해당하는 함수(콜백함수)를 실행한다.
```js
var txt = "";
var numbers = [45, 4, 9, 16, 25];
numbers.forEach(myFunction);

function myFunction(value, index, array) {
  txt = txt + value + "<br>";
}
// txt 에 배열의 모든 요소를 저장.
```

**[참고]** array의 iteration 메서드를 통해 부르는 function은 value, index, array(해당 array 자신)을 arguments로 취한다. 위 예제의 경우 value만 사용하고 있는데, 이 경우 myFunciton의 파라미터 값으로 value만 받아서 사용해도 된다.
{: .notice--info}


* map() : 각 배열 요소에 function을 수행하여 새 배열을 만들며, _값이 없는 배열 요소에 대한 함수는 실행하지 않는다._ array의 _원본을 변경하지 않고_ 새로운 array를 생성한다.
```js
var numbers1 = [45, 4, 9, 16, 25];
var numbers2 = numbers1.map(myFunction);

function myFunction(value, index, array) {
  return value * 2;
}
```

* filter() : 각 배열 요소에 function의 조건을 만족하는 요소로 이루어진 새 배열을 만든다.
```js
var numbers = [45, 4, 9, 16, 25];
var over18 = numbers.filter(myFunction);

function myFunction(value, index, array) {
  return value > 18;
}
```


* reduce() : 배열 요소에 대해 함수를 실행하여 단일 값을 생성한. 원본 값을 변경하지 않는다. _배열의 합을 구할때 사용._ 좌->우로 실행되며, reduceRight() 메서드는 마지막 인덱스부터 시작한다.
```js
var numbers1 = [45, 4, 9, 16, 25];
var sum = numbers1.reduce(myFunction); // 99

function myFunction(total, value, index, array) {
  return total + value;
}
```
**[참고]** total은 초기값 || 이전에 반환된 값
{: .notice--info}



* every() : 배열의 모든 요소가 지정조건을 통과하는지 체크한다.
```js
var numbers = [45, 4, 9, 16, 25];
var allOver18 = numbers.every(myFunction);  // false

function myFunction(value, index, array) {
  return value > 18;
}
```

* find() / findInder() : 해당 조건을 통과하는 요소의 값/인덱스를 반환한다. (해당하는 요소가 여러개일 경우, 가장 먼저 만족하는 요소를 리턴)
```js
var numbers = [4, 9, 16, 25, 29];
var first = numbers.find(myFunction); // 25

function myFunction(value, index, array) {
  return value > 18;
}
```

```js
var numbers = [4, 9, 16, 25, 29];
var first = numbers.findIndex(myFunction); // 3

function myFunction(value, index, array) {
  return value > 18;
}
```


> w3schools - JS Array Method
> : <https://www.w3schools.com/js/js_array_methods.asp>

> w3schools - JS Array Sort
> : <https://www.w3schools.com/js/js_array_sort.asp>
