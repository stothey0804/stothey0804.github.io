---
title: javascript - window 객체, HTML DOM
categories:
- Javascript
toc: true
---

## Browser Object

브라우저에는 브라우저 객체가 내장되어있는데 브라우저 객체의 계층적 구조를 BOM(Browser Object Model)이라 하며, 그중 가장 최상위 객체가 window 객체다.

```js
window.setTimeout()
setTimeout() //window는 전역객체이므로, 생략 가능.
```

![BOM 계층구조]({{ site.url }}{{ site.baseurl }}/assets/images/js-1.png)


* screen : 사용자의 모니터 정보(속성)을 제공하는 객체 ex.모니터 너비,높이...
* location : 사용자 브라우저와 관련된 속성과 메서드 제공. ex. location.href; location.reload();
* history : 사용자가 방문한 사이트의 기록
* navigator : 현재 방문자가 사용하는 브라우저 정보와 운영체제 정보를 제공하는 객체


### winodw 객체의 메서드 종류

|종류|설명|
|---|---|
|open("url","새 창 이름","새 창 옵션")|URL페이지를 새 창으로 나타낸다.|
|alert(data)|경고창을 나타내어 데이터의 값을 보여준다.|
|prompt("질문","답변")|사용자에게 답변을 받는 질의응답창을 나타낸다.|
|confirm("내용")|내용에 따른 확인/취소 창을 나타낸다. 사용자 선택에 따라 true(확인) 또는 false(취소)를 리턴한다.|
|moveTo(x,y)|지정한 새 창의 위치 이동|
|resizeTo(w,h)|지정한 새 창의 크기를 변경|
|setInterval(function(){ ... }, ms)|ms 간격으로 함수를 호출하여 실행한다.|
|setTimeOut(function(){ ... }, ms)|**한 번** ms간격으로 함수를 호출하여 실행 |


>출처 : Do it! 자바스크립트 + 제이쿼리 입문

#### setInterval / setTimeout

일정 시간 간격으로 코드를 실행하는 메서드로, setInterval()은 반복 실행하고, setTimeOut()은 한 번만 실행한다.
clearInterval() clearTimeOut() 메서드로 해제할 수 있다.

```js
var intv = setInterval(function(){ num++ }, 1000) // 1초당 num을 증가
clearInterval(intv); // 해제
var tim = setTimeOut(function(){ num++ }, 5000) // 5초 후에 num 증가
clearTimeOut(tim);  // 해제
```

**[참고] setTimeout의 실행순서**

setTimeout의 실행은 비동기(asynchronous)로 실행되어 동기적인 다른 실행이 끝나야 실행된다. 다음 예제를 참고할 것.

```js
function run() {
    console.log("run start");
    setTimeout(function() {
        var msg = "hello";
        console.log(msg);  // 즉시 실행되지 않음.
    }, 1000);
    console.log("run end");
}
console.log("start");
run();
console.log("end");
// start -> run start -> run end -> end -> hello 가 출력된다.
```

> edwith 부스트코스
> : <https://www.edwith.org/boostcourse-web/lecture/16698/>



## HTML DOM (Document Object Model)

HTML의 element는 DOM Tree로 저장된다. 이 DOM Tree를 쉽게 탐색할 수 있도록 브라우저에서 제공하는 메서드가 DOM API다.
스크립트를 사용해 동적으로 접근하여 문서의 내용/구조/스타일을 변경할 수 있다.

<img src="https://www.w3schools.com/js/pic_htmltree.gif">

* Node : DOM Tree는 Node의 집합이다.
1. 요소 노드 Element
  - ``<body>``, ``<p>`` 와 같은 태그 요소
2. 텍스트 노드 Text
  - 노드 안의 text 부분
3. 속성 노드 Attribute
  - 요소태그의 속성 부분
4. comment 노드 (주석)

```html
<p title='the title'>타이틀</p>
// 요소노드 - p
// 속성노드 - title
// 텍스트노드 - 타이틀
```

> W3School - HTML DOM
> : <https://www.w3schools.com/js/js_htmldom.asp>

### DOM 기초 예제

**HTML**

```html
<body>
<input type="button" value="Change this document." onclick="change()">
<h2>Header</h2>
<p>Paragraph</p>
</body>
```

**Javascript**

```javascript
 function change() {
    // document.getElementsByTagName("H2") 는 <h2> 엘리먼트의
    // NodeList를 반환(return)합니다. 0부터 시작합니다.
    
    var header = document.getElementsByTagName("H2").item(0);
    // 헤더의 첫번째 자식 노드는 Text 노드입니다.
    header.firstChild.data = "A dynamic document";
    // 이제 헤더는 "A dynamic document"가 되었습니다.
    
    var para = document.getElementsByTagName("P").item(0);
    para.firstChild.data = "This is the first paragraph.";
    
    // 두번째 문단을 만들기 위한 새로운 Text 노드를 만들었습니다. 
    var newText = document.createTextNode("This is the second paragraph.");
    // 두번째 문단을 만들기 위한 새로운 p 엘리먼트를 만들었습니다.
    var newElement = document.createElement("P");
    // 새로운 p 엘리먼트에 텍스트를 넣어 문단을 만듭니다.
    newElement.appendChild(newText);
    // 생성한 문단을 HTML 문서 마지막에 넣기 위해
    // (para의 부모인) HTML BODY에 append합니다.
    para.parentNode.appendChild(newElement);
  }
```

### Element


### Event Object


https://www.w3schools.com/jsref/default.asp