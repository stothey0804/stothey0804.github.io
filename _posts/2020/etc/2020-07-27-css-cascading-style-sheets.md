---
title: CSS - 개념, 선택자
tags:
- css
categories:
- etc
toc: true
---

CSS (Cascading Style Sheets)란,

브라우저에서 웹페이지의 외형을 결정하는 선언형 언어로, 선택한 요소에 CSS 스타일 선언을 적용해 화면에 표현한다.
CSS는 보통 HTML 요소의 스타일을 결정하지만, SVG와 XML 등 다른 마크업 언어에도 사용할 수 있습니다.
하나의 CSS 규칙은 선택자에 연결된 속성과 값 세트로 구성된다.

## CSS의 선언
### 1. External
```
<link rel='stylesheet' type='text/css' href='mystyle.css'>
```
### 2. Internal
```
<head>
	<style>
		body{
			background-color: green;
		}
	</style>
</head>
```
### 3. inline

```
<body style='background-color : green'>
	...
</body>
```
- - -
## CSS 선택자(Selector)


선택자는 특정 HTML 태그를 선택할 떄 사용하는 기능이다.
선택자 {스타일 속성: 스타일 값;} 형태로 선언한다.

```
h1{ // 선택자
	color: red; // 속성 : 값;
	background-color: orange;
}
```

<br>

### 선택자의 종류
*  \* 전체
전체 태그에 적용하는 속성

* __태그명__
div, h1 등 해당하는 태그 별로 적용

* __#__ id
아이디 속성을 가지는 태그를 선택

* __.__ class
tag와 id가 아닌 것을 그룹핑할 때 (해당 클래스를 가진 태그에 적용)

* **속성 선택자**
특정 속성을 가진 HTML 태그를 선택
|선택자 형태|설명|
|---|---|
|선택자\[속성]|특정한 속성이 있는 태그를 선택|
|선택자\[속성=값]|특정한 속성 안의 값이 특정값과 같은 문서 객체 선택|

	```
	<head>
		<title>CSS3 Selector</title>
		<style>
			h1[class]{  background-color:red;  }
			h1[class=select2]{  background-color: blue;  }
		</style>
	</head>
	<body>
		<h1>111111111</h1>
		<h1 class='select'>111111111</h1>
		<h1 class='select2'>111111111</h1>
	</body>
```

* **부분속성 선택자**
정확한 속성값이 아닐때 기호에 따라 적용결과에 차이가 있다.

	* \[class~="bar"] : class 속성의 값이 공백으로 구분한 "bar" 단어가 포함되는 요소 선택
	* \[class^="bar"] : class 속성의 값이 "bar"로 시작하는 요소 선택
	* \[class$="bar"] : class 속성의 값이 "bar"로 끝나는 요소 선택
	* \[class*="bar"] : class 속성의 값이 "bar" 문자가 포함되는 요소 선택

* **선택자A 선택자B**
선택자A의 ___모든___ 하위 선택자B를 선택

	```
	<head>
		<title>CSS3 Selector</title>
		<style>
			#header h1 { color: red; }
			#section h1, #section h2 {  color: orange;  }
		</style>
	</head>
	<body>
		<div id="header">
			<h1>lorem ipsum</h1>
			<div id="nav">
				<h1>Navigation</h1>  <!--모든 하위클래스에 적용된다 -->
			</div>
		</div>
		<div id="section">
			<h1 class="title">lorem ipsum</h1>
			<h2 class="title">lorem ipsum</h2>
			<p>Lorem ipsum dolor sit amet,</p>
		</div>  
	</body>
	```

* **선택자A > 선택자B**
선택자A의 바로 아래에 있는 하위 선택자 B를 선택

	```
	<head>
		<title>CSS3 Selector</title>
		<style>
			#header > h1 { color: red; }
			#section > h1, #section > h2 {  color: orange;  }
		</style>
	</head>
	<body>
		<div id="header">
			<h1>lorem ipsum</h1>
			<div id="nav">
				<h1>Navigation</h1>  <!-- 바로 하위의 선택자가 아니므로 적용되지 않는다. -->
			</div>
		</div>
		<div id="section">
			<h1 class="title">lorem ipsum</h1>
			<h2 class="title">lorem ipsum</h2>
			<p>Lorem ipsum dolor sit amet,</p>
		</div>  
	</body>
	```

* **선택자A + 선택자B**
바로 뒤에 인접한 형제 선택자

* **선택자A ~ 선택자B**
뒤에 위치하는 모든  형제 선택자

	```
	<head>
		<title>CSS3 Selector</title>
		<style>
			h1 + h2 { color: red }
			h1 ~ h2 { background-color: orange;}
		</style>
	</head>
	<body>
		<h1>Header - 1</h1>
		<h2>Header - 2</h2>
		<h2>Header - 2</h2>
		<h2>Header - 2</h2>
	</body>
	```

* **반응선택자**
:active - 사용자가 마우스로 클릭한 태그
:hover - 사용자가 마우스를 올린 태그

* **상태선택자** (입력양식 상태)
:checked - 체크상태의 input (radio, checkbox, option)
:focus - 초점이 맞추어진 input
:enabled - 사용 가능한 input
:disabled - 사용 불가능한 input

	```
	<head>
		<title>CSS3 Selector</title>
		<style>
			option:checked {  background-color: yellow;  }
			input[type="checkbox"]:checked {  width: 20px; height: 20px; }
			input[type="radio"]:checked { margin-left: 25px }
		</style>
	</head>
	<body>
		<h3>option 요소</h3>
		<select>
			<option value="20">20대</option>
			<option value="30">30대</option>
			<option value="40">40대</option>
		</select>
		<form action="url" method="post">
			<h3>Type = "checkbox"</h3>
			<input type="checkbox" name="food" value="fruit">과일<br>
			<input type="checkbox" name="food" value="sea">해조류<br>
			<input type="checkbox" name="food" value="meat">육류

			<h3>Type = "radio"</h3>
			<input type="radio" name="media" value="ie">Internet Explorer<br>
			<input type="radio" name="media" value="gc">Google Chrome<br>
			<input type="radio" name="media" value="ff">FireFox
		</form>
	</body>
	```


* **구조 선택자** (특정 위치의 태그를 선택)
:first-child - 형제 관계 중 첫번째
:last-child - 형제 관계 중 마지막
:nth-child(수열) - 형제 관계중 앞에서 수열번째
:nth-last-child(수열) - 형제 관계중 뒤에서 수열번째

	```
	<head>
		<title>CSS3 Selector</title>
		<style>
			li:first-child { color: red; }
			li:last-child {  color: blue; }
			li:nth-child(2n) { background-color: yellow; }
			li:nth-last-child(2n+1) { background-color: gray; }
			li:nth-child(2) { background-color: green; }
		</style>
	</head>
	<body>
		<ul>
			<li>first</li>
			<li>second</li>
			<li>third</li>
			<li>fourth</li>
			<li>fifth</li>
			<li>sixth</li>
			<li>seventh</li>    
		</ul>
	</body>
	```

* **형태구조 선택자**

:first-of-type
:last-of-type
:nth-of-type(수열)
:nth-last-of-type

**[참고]** 일반 구조 선택자는 순서를 정확히 해야하지만, 형태구조 선택자는 type을 만나는 순서대로 적용된다.
{: .notice}

	```
<head>
<style>
  /*div p:first-child { color:red; } */  /* div하위에 p가 첫번째로 존재하지 않아 적용X */
  div p:first-of-type { color:red; }  /* type만드로 찾기 때문에 적용가능 */
</style>
</head>
<body>
  <div>
    <div>text1</div>
    <p>text2</p>
    <p>text3</p>
  </div>
</body>
	```

* 그 외 선택자

::first-letter - 첫번째 글자
::first-line - 첫번째 줄
a:visited - 방문한 적 있는 링크
:not - 해당 선택자가 아닌 것에 적용


>선택자 연습 사이트
> : <h:ttp://flukeout.github.io/>

- - -
## CSS 적용 우선순위

### 캐스케이딩(Cascading)

cascading은 폭포가 내려오는 모양처럼 단계적이라는 의미이며,
스타일 규칙은 cascading의 규칙에 따라 요소에 적용된다.

#### cascading 규칙
##### 1. 중요도(important)와 출처

 * **important** 가 선언되면 가장 높은 우선권을 갖는다.

```
p#page { color: red !important; }
```

```
<p id="page" style="color:blue">Lorem impusm dolor sit.</p> // red 적용
```

	* 출처
	제작자, 사용자, 사용자 에이전트(user agent)로 구분하며, 현재는 사용자 작성 스타일을 지원하지 않는 추세이므로 생략.
	제작자 지정 스타일 > 사용자 에이전트(브라우저 내장 CSS)

##### 2. 구체성

 같은 요소를 선택하는 서로 다른 규칙들이 상반된 스타일을 가지고 있을 때, 적용되는 규칙을 구체성이라 한다.

```
 0, 0, 0, 0
```

 구체성은 4개의 숫자 값으로 이루어져 있으며 좌측의 숫자가 클 수록 높은 구체성을 가진다.

 * 1, 0, 0, 0 : 인라인 스타일
 * 0, 1, 0, 0 : 선택자에 있는 모든 id 속성값
 * 0, 0, 1, 0 : 선택자에 있는 모든 class 속성값, 기타 속성, 가상 클래스
 * 0, 0, 0, 1 : 선택자에 있는 모든 요소, 가상 요소
 * 전체 선택자는 0, 0, 0, 0을 가진다.
 * 조합자는 구체성에 영향을 주지 않는다. (>, + 등)


```
h1 { ... }  /* 0,0,0,1 */
body h1 { ... }  /* 0,0,0,2 */
.grape { ... }  /* 0,0,1,0 */
*.bright { ... }  /* 0,0,1,0 */
p.bright em.dark { ... }  /* 0,0,2,2 */
#page { ... }  /* 0,1,0,0 */
div#page { ... }  /* 0,1,0,1 */
```


 > edwith 부스트코스
 > : <https://www.edwith.org/boostcourse-ui/lecture/33515/>

##### 3. 선언 순서

 뒤에 선언된 규칙일 수록 우선한다.

 > edwith 부스트코스
 > : <https://www.edwith.org/boostcourse-ui/lecture/33517/>

 > MDN
 > : <https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance>


- - -


## CSS 상속
일반적으로 상속되는 속성

```
visibility, opacity, font, color, line-height, text-align, white-space
```

상속되지 않는 속성
```
width, height, margin, padding, border, box-sizing, display, background,
vertical-align, text-decoration, position, top, right, bottom, left,
z-index, overflow, float
```
