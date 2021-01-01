---
title: \[rails] 레일즈 모델 기본 - 액티브 레코드를 사용한 모델 생성
categories:
- Ruby on rails
tags:
- ruby
toc: true
---

## 목표
- 레일즈의 모델 기본 개념을 배운다.

## 1. O/R 맵퍼

O/R(Object/Relational) 맵퍼는 관계형 데이터베이스와 객체 사이를 연결시켜주는 라이브러리다.
실제 데이터 베이스와 객체의 구조적인 차이를 객체 관계 불일치라 하는데, O/R 맵퍼가 잘못된 매칭을 제거 해주고, **액티브 레코드**에서는 데이터베이스의 테이블 하나에 대하여 모델 클래스를 생성하며, 객체의 속성은 테이블의 필드를 갖게되는 구조이다.

## 2. 연결 및 모델 생성

액티브 레코드를 사용해 데이터베이스에 연결하려면 먼저 `config/database.yml`에 설정을 해준다.

**모델 생성 명령어**

```
rails generate model name field:type [...] [options]
```

- name : 모델명
- field : 필드명
- type : string, integer, date, Boolean 등 타입

### 모델 Association 이름 규칙

|종류|설명|예|
|---|---|---|
|모델 클래스|첫 글자 대문자, 단수형|Book|
|모델 클래스 파일명|첫글자 소문자, 단수형|book.rb|
|테이블|첫글자 소문자, **복수형**|books|
|테스트 스크립트|xxxx_test.rb(소문자,단수형)|book_test.rb|

## 3. 마이그레이션 파일로 테이블 생성

레일즈는 테이블 생성 및 수정에 **마이그레이션(Migration)** 기능을 사용한다. 마이그레이션(Migration)이란 정리하면 테이블 레이아웃을 생성 또는 변경하기 위한 구조이며 테이블 관련 작업을 반자동화한다.

마이그레이션 파일은 모델을 생성하는 `rails generate` 명령어를 실행할 때 자동 생성 된다.

**마이그레이션 파일 실행**

```
rake db:migrate
```

마이그레이션 파일을 실행할때는 매개변수 없이 위 명령어만 실행하면 알아서 읽고 생성해준다.

**[참고]** Rails에서 테이블을 생성하면, id, created_at, updated_at 필드가 자동 생성된다.
{: .notice}	

## 4. 기본 데이터 추출

실제로 액티브 레코드를 사용하여 books 테이블의 데이터를 조회하여 출력하는 예제를 간단히 살펴 본다.

**[참고]** 자동 생성된 Book 클래스 (book.rb) 를 확인하면 내부에 아무 코드도 없는데, 부모클래스인 `ActiveRecord::Base` 클래스가 기본 접근을 위한 기능을 제공하고 있어서 사용이 가능하다. 모델에 유효성 검사 및 사용자 기능을 구현하는 방법은 추후 살펴보기로 한다.
{: .notice}	

### 4.1. 컨트롤러

hello_controller.rb

```ruby
class HelloController < ApplictationController
  ...
  def list
    @books = Book.all
  end
end
```

### 4.2. 뷰(템플릿)

hello/list.html.erb

```ruby
...
<% @books.each do |book| %>
  <tr>
    <td><%= book.isbn %></td>
    <td><%= book.title %></td>
    <td><%= book.price %>원</td>
    <td><%= book.publish %></td>
    <td><%= book.published %></td>
    <td><%= book.cd %></td>
  </tr>
<% end %>
...
```

`http://localhost:3000/hello/list` 에 접속하면 books 테이블의 내용을 조회할 수 있다.