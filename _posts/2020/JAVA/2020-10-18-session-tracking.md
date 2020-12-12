<<<<<<< HEAD
---
title: \[Spring/Servlet] 세션 트래킹 - 쿠키/세션 사용하기 
tags:
- Servlet
categories:
- Java
toc: true
toc_sticky: true
---

프로젝트를 진행하다가 한번 정리해야할 것 같아서
세션 트래킹에 관련하여 간단하게 정리하려고한다.




# 세션 트래킹이란?

HTTP 프로토콜은 서버-클라이언트 통신시 stateless 방식으로 통신하기 때문에
이전 웹 페이지에서 어떤 작업을 수행했는지 알 수 없다...

그렇기 때문에 각 페이지의 상태나 정보를 다른 페이지(서블릿)과 연결해줄 필요가 있는데,
이 방법을 세션 트래킹이라 한다.


## 세션 트래킹 방법

세션 트래킹을 하는 방법은 다음과 같다.

* `<hidden>` 태그 사용
```html
<input type='hidden' name='id' value='test'>
// id = test 라는 값을 form 전송시 함께 보내준다.
```

* URL Rewriting : GET방식으로 정보를 붙여 보내기
`ex) {URI}?id=test&...`

* 쿠키 : 클라이언트PC 쿠키파일에 정보 저장 - 웹사이트 내 서비스 (도메인)

* 세션 : 서버 메모리에 정보를 저장한 후 공유 - 한 브라우저가 접근하는 서비스 

- - -


# 쿠키 API

`javax.servlet.http.Cookie`

- 쿠키는 HTTP 헤더에 추가되어 키-값 쌍의 형태로 전달된다.
- 쿠키는 클라이언트의 PC에 저장된다.
- 사용자가 쿠키의 내용을 볼 수 있고 맘대로 수정도 할 수 있다.
- 보안성이 떨어지므로 중요한 정보는 쿠키에 저장하지 않는다.
- 브라우저별로 쿠키의 수와 용량에 제한이 있다.


## 쿠키의 생성과 삭제

`HttpServletResponse`의 `addCookie()` 메서드를 이용해 쿠키를 전송한다.

### 쿠키 생성 

```java
Cookie c = new Cookie("cookieTest", URLEncoder.encode("내용","utf-8")); // 한글일 경우
c.setMaxAge(60*60*24);   // 초단위, 1일설정
response.addCookie(c) ;  
```

### 쿠키 삭제

```java
Cookie c = new Cookie("cookieTest", null)
c.setMaxAge(0);          // 0으로 설정시 삭제
response.addCookie(c) ;  
```


## 쿠키 조회

### Servlet 쿠키 조회하기

`HttpServletResponse`의 `getCookie()` 메서드를 이용해 쿠키를 서버로 가져온다.

```java
Cookie[] allValues = request.getCookie();
```


### Spring @CookieValue 를 사용하여 읽기

Spring MVC의 @CookieValue 애너테이션을 사용하여 읽는 방법.

```java
public String method(@CookieValue(value="cookieTest", defaultValue="1", required=true) String value) {
    ...
}
```

**required가 true**일 경우, 쿠키가 없을 시 java.lang.IllegalStateException가 발생한다고 한다.
따라서 **defaultValue**를 설정하거나, required=**false**로 설정하는 것이 좋다.


- - -


# 세션 API

`javax.servlet.http.HttpSession`

- 세션은 서버에 저장된다.
- 클라이언트 세션 저장소의 내용을 볼 수 없고 수정도 할 수 없다.
- 클라이언트는 세션키를 조회할 수 있으며, 세션키는 쿠키를 통해 전달된다.
- 보안성이 좋다. (login시 사용)

## 세션 생성 과정

1. 브라우저 접속
2. 서버에서 세션 객체 생성
3. 서버 --- 세션id --> 클라이언트 응답
4. 브라우저가 세션쿠키 저장 (JsessionId)

HttpServletResponse 의 getSession() 메서드를 호출해서 HttpSession 객체를 생성

```java
HttpSession session = request.getSession();
```

[**참고**]
- `.getSession()` : 기존객체가 없을 경우 새로 생성 (ex: 로그인 시)
- `.getSession(false)` : 기존객체가 없을 경우 null 반환


## 세션 메서드

```java
session.getId()         // 세션에 할당된 식별자(세션키)를 String으로 반환
session.isNew()         // 최초로 생성된 세션인지 판별
session.Invalidate()    // 생성된 생성 소멸 (ex: 로그아웃 시)
session.setMaxInactiveInterval(60*60*24)    // 세션 유지시간 초단위 설정
=======
---
title: \[Spring/Servlet] 세션 트래킹 - 쿠키/세션 사용하기 
tags:
- Servlet
categories:
- Java
toc: true
toc_sticky: true
---

프로젝트를 진행하다가 한번 정리해야할 것 같아서
세션 트래킹에 관련하여 간단하게 정리하려고한다.




# 세션 트래킹이란?

HTTP 프로토콜은 서버-클라이언트 통신시 stateless 방식으로 통신하기 때문에
이전 웹 페이지에서 어떤 작업을 수행했는지 알 수 없다...

그렇기 때문에 각 페이지의 상태나 정보를 다른 페이지(서블릿)과 연결해줄 필요가 있는데,
이 방법을 세션 트래킹이라 한다.


## 세션 트래킹 방법

세션 트래킹을 하는 방법은 다음과 같다.

* `<hidden>` 태그 사용
```html
<input type='hidden' name='id' value='test'>
// id = test 라는 값을 form 전송시 함께 보내준다.
```

* URL Rewriting : GET방식으로 정보를 붙여 보내기
`ex) {URI}?id=test&...`

* 쿠키 : 클라이언트PC 쿠키파일에 정보 저장 - 웹사이트 내 서비스 (도메인)

* 세션 : 서버 메모리에 정보를 저장한 후 공유 - 한 브라우저가 접근하는 서비스 

- - -


# 쿠키 API

`javax.servlet.http.Cookie`

- 쿠키는 HTTP 헤더에 추가되어 키-값 쌍의 형태로 전달된다.
- 쿠키는 클라이언트의 PC에 저장된다.
- 사용자가 쿠키의 내용을 볼 수 있고 맘대로 수정도 할 수 있다.
- 보안성이 떨어지므로 중요한 정보는 쿠키에 저장하지 않는다.
- 브라우저별로 쿠키의 수와 용량에 제한이 있다.


## 쿠키의 생성과 삭제

`HttpServletResponse`의 `addCookie()` 메서드를 이용해 쿠키를 전송한다.

### 쿠키 생성 

```java
Cookie c = new Cookie("cookieTest", URLEncoder.encode("내용","utf-8")); // 한글일 경우
c.setMaxAge(60*60*24);   // 초단위, 1일설정
response.addCookie(c) ;  
```

### 쿠키 삭제

```java
Cookie c = new Cookie("cookieTest", null)
c.setMaxAge(0);          // 0으로 설정시 삭제
response.addCookie(c) ;  
```


## 쿠키 조회

### Servlet 쿠키 조회하기

`HttpServletResponse`의 `getCookie()` 메서드를 이용해 쿠키를 서버로 가져온다.

```java
Cookie[] allValues = request.getCookie();
```


### Spring @CookieValue 를 사용하여 읽기

Spring MVC의 @CookieValue 애너테이션을 사용하여 읽는 방법.

```java
public String method(@CookieValue(value="cookieTest", defaultValue="1", required=true) String value) {
    ...
}
```

**required가 true**일 경우, 쿠키가 없을 시 java.lang.IllegalStateException가 발생한다고 한다.
따라서 **defaultValue**를 설정하거나, required=**false**로 설정하는 것이 좋다.


- - -


# 세션 API

`javax.servlet.http.HttpSession`

- 세션은 서버에 저장된다.
- 클라이언트 세션 저장소의 내용을 볼 수 없고 수정도 할 수 없다.
- 클라이언트는 세션키를 조회할 수 있으며, 세션키는 쿠키를 통해 전달된다.
- 보안성이 좋다. (login시 사용)

## 세션 생성 과정

1. 브라우저 접속
2. 서버에서 세션 객체 생성
3. 서버 --- 세션id --> 클라이언트 응답
4. 브라우저가 세션쿠키 저장 (JsessionId)

HttpServletResponse 의 getSession() 메서드를 호출해서 HttpSession 객체를 생성

```java
HttpSession session = request.getSession();
```

[**참고**]
- `.getSession()` : 기존객체가 없을 경우 새로 생성 (ex: 로그인 시)
- `.getSession(false)` : 기존객체가 없을 경우 null 반환


## 세션 메서드

```java
session.getId()         // 세션에 할당된 식별자(세션키)를 String으로 반환
session.isNew()         // 최초로 생성된 세션인지 판별
session.Invalidate()    // 생성된 생성 소멸 (ex: 로그아웃 시)
session.setMaxInactiveInterval(60*60*24)    // 세션 유지시간 초단위 설정
>>>>>>> a937bcd5bda086a5cff6b5e97a82ecae4ebcc3f8
```