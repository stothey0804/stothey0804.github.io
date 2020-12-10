---
title:  HTTP, 웹 서버 동작방식
tags:
- web
categories:
- BE
toc: true
---

## HTTP

HTTP란, **서버와 클라이언트가 인터넷상에서 데이터를 요청/응답(request/response)을 통해 주고받기 위한 프로토콜(protocol)**이다.
- 프로토콜 = 서로 정해놓은 규약


### HTTP 메시지 (요청/응답)

HTTP 메시지는 서버와 클라이언트 간에 데이터가 교환되는 방식이다.
**요청(request)**은 클라이언트가 서버로 전달해서 서버의 액션이 일어나게끔 하는 메시지고,
**응답(response)**은 요청에 대한 서버의 답변이다.

<img src="https://mdn.mozillademos.org/files/13827/HTTPMsgStructure2.png">

> 출처 : MDN <a href="https://developer.mozilla.org/ko/docs/Web/HTTP/Messages">https://developer.mozilla.org/ko/docs/Web/HTTP/Messages</a>


### 주요 응답 코드

|코드|메시지|내용|
|---|---|---|
|200|OK|정상|
|204|No Content|문서 없음|
|400|Bad Request|잘못된 요청|
|403|Fobbiden|권한 없음|
|**404**|Not Found|찾을 수 없음|
|405|Method Not Allowed|허용하지 않는 메소드|
|**500**|Internal Server Error|서버에러|
|503|Service Unavailable|서버 한계 초과|

실제로 자주 볼 수 있는 오류코드는 404와 500다.. 🙃
404는 매핑이 잘 안됐을 때, 500는 어딘가 연산이 잘못됐을 때 우리를 반겨준다.



## 클라이언트-서버 구조

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c9/Client-server-model.svg/1920px-Client-server-model.svg.png">

> 출처: 위키백과 클라이언트-서버 모델

* 클라이언트(Client) : 서비스를 제공하는 서버에게 정보를 요청하여 응답받은 결과를 사용하는 주체라고 할 수 있다. 대표적으로 웹 브라우저를 뜻하는데 사용자를 가리키기도하고 단말기를 가리키기도 한다. 
* 서버(Server) : 네트워크를 통해 클라이언트에 정보나 서비스를 제공하는 컴퓨터 혹은 SW를 말한다. 웹 서버 SW는 대표적으로 Apache, Nginx 등이 있다. 


## WAS (Web Application Server)

WAS는 일종의 미들웨어로 웹 애플리케이션과 서버 환경을 만들어 동작시키는 기능을 제공하는 소프트웨어 프레임워크이다.
우리가 아는 Tomcat이 WAS라고 생각하면 된다.

- WAS는 동적 서버 컨텐츠를 수행.
- 클라이언트와 DBMS 사이에서 비즈니스 로직을 동작하는 미들웨어.
* DBMS(DataBase Management System) : 다수의 사용자가 DB 내의 데이터에 접근할 수 있도록 해주는 SW.

#### [참고] 웹 서버 vs WAS
-  WAS도 보통 자체적으로 웹 서버 기능을 내장하고 있다.
- 현재는 WAS가 가지고 있는 웹 서버도 정적인 콘텐츠를 처리하는 데 있어서 성능상 큰 차이가 없다.
- 규모가 커질수록 웹 서버와 WAS를 분리한다.
- 자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성을 위해 웹서버와 WAS를 대체로 분리한다.

> 출처 <a href="https://www.edwith.org/boostcourse-web/lecture/16666/">https://www.edwith.org/boostcourse-web/lecture/16666/</a>