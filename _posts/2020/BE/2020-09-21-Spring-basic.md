---
title: Spring - Spring Framework와 Spring MVC
categories:
- BE
toc: true
toc_sticky: true
---

## Spring Framework

스프링 프레임워크는 스프링 프로젝트의 주요 모듈중 하나이며,
앱을 개발할때 기반이 되는 프레임워크로 DI, AOP, MVC, ORM 기능을 제공한다.
약 20개 모듈로 구성되었으며 개발시 필요한 모듈만 가져다 사용할 수 있다.

<img src="https://docs.spring.io/spring-framework/docs/4.3.29.RELEASE/spring-framework-reference/htmlsingle/images/spring-overview.png">

> 스프링 프레임워크 구조

#### Spring 주요 모듈 목록

|프로젝트|설명|
|---|---|
|Spring Framework|App 개발시 기반이 되는 FW. DI,AOP,MVC,ORM기능을 제공.|
|Spring Data|Data 연동을 위한 API를 제공, RDBMS와 NoSQL과 연동을 적은 양의 코드로 처리할 수 있음.|
|Spring Security|App의 보안을 간단한 설정과 코드로 구현할 수 있음.|
|Spring Batch|데이터 처리, 흐름제어, 실패 재처리 등 배치 처리 App이 필요로 하는 기능을 제공.|
|Spring Integration|시스템 간 연동을 위한 메시징 FW를 제공.|
|Spring Social|소셜 네트워크 연동을 위한 기능을 제공.|



### Spring 제공 기능

* Beans : 스프링 컨테이너를 이용해서 객체를 생성하는 기본 기능

* Context : 애플리케이션의 각 기능을 하는 Bean에 대한 접근방법 지원

* DAO : JDBC 기능을 편리하게 사용

* ORM : 마이바티스 같은 DB Mapping 관련 FW와 연동

* AOP : 관점 지향 기능을 제공

* Web : 웹 App 개발에 필요한 기능을 제공

* WebMVC : MVC 구현에 관련된 기능을 제공

* tx : 트랜젝션 처리를 위한 추상 레이어 제공

* WebSocket : Spring MVC에서 웹 소켓 연동을 처리


### Spring Framework의 특징

#### POJO 방식 Framework

 Plain Old Java Object의 준말로,
특정 자바 모델이나 프레임워크에 종속되지 않는 자바 오브젝트를 뜻한다.
POJO방식 프레임워크인 Spring Framework는 특정 규약과 환경에 종속되지 않아 테스트 하기 용이하며, 로우레벨 코드와 비즈니스 코드가 분리되어 깔끔한 코드 작성이 가능하다.


#### IoC(Inversion of Control) - 제어 역행 기술

자바코드로 직접 처리했던 객체의 생성을 스프링 컨테이너가 대신 처리하여 객체 사이의 의존관계가 명시되지 않으므로 낮은 결합도를 유지하며, 유지보수가 편리하다.

#### DI(Dependecy Injection) - 의존성 주입

IoC를 지원하는 방법으로, DI는 컨테이너가 객체사이의 의존관계를 스프링 설정 파일에 등록한 정보를 바탕으로 직접 처리해주는 것을 의미한다.


#### AOP(Aspect Oriented Programming) 

관점지향 프로그래밍은 비즈니스 메소드를 개발할 때 핵심 로직과 반복해서 등장하는 공통로직을 분리함으로써 응집도 높은 개발을 할 수 있도록 지원한다.


- - -

## Spring MVC

