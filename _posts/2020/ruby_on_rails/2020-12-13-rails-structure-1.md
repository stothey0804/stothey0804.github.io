---
title: \[rails] 레일즈 구조 파헤치기
categories:
- Ruby on rails
tags:
- ruby
toc: true
---

## 목표

레일즈의 기본적인 구조를 파악한다.


## 레일즈 프로젝트 구조

### 요약

|파일/폴더|목적|
|---|---|
|Gemfile|이 파일은 여러분의 레일즈 어플리케이션에게 필요한 젬의 의존성 정보를 기술하는데 사용.|
|README|이 파일은 어플리케이션을 위한 짧막한 설명이다. 설치, 사용 방법 기술에 쓰임.|
|Rakefile|이 파일은 터미널에서 실행할 수 있는 배치잡들을 포함.|
|**app/**|어플리케이션을 위한 컨트롤러, 모델, 뷰를 포함. |
|**config/**|어플리케이션의 실행 시간의 규칙, 라우팅, 데이터베이스 등 설정을 저장합.|
|config.ru|랙(Rack) 기반의 서버들이 시작할때 필요한 설정.|
|**db/**|현재 데이터베이스의 스키마를 볼 수 있다.(데이터베이스 마이그레이션으로 잘 알려져 있다.)|
|doc/|어플리케이션에 대한 자세한 설명 문서|
|lib/|라이브러리|
|log/|로그|
|public/|외부에서 다이렉트로 접근할 수 있는 유일한 폴더. (에러 페이지 등)|
|script/|레일즈 스크립트를 포함한다. 어플리케이션을 실행시키거나, 배포, 실행 관련한 스크립트.|
|test/|유닛 테스트, 픽스쳐, 그와 다른 테스트 도구.|
|tmp/|임시 파일|
|vendor/|서드 파티 코드들을 위한 공간. 일반적인 레일즈 어플리케이션은 루비 젬과 레일즈 소스-프로젝트 내에 설치시-와 미리 패키징된 추가 플러그인들이 위치한다. `bundler`가 위치함. |

🔗 <a href="http://rubykr.github.io/rails_guides/getting_started.html#Starting-up-the-Web-Server">레일즈 시작하기</a>

- - -



https://github.com/pureugong/rails-style-guide/blob/master/README-koKR.md


### 1. 설정(configuration)

설정부분은 `config/` 폴더 아래에 위치한다.

사용자 초기 설정은 `initializers`에 저장되며, 어플리케이션이 처음 구동될 때 실행된다.
모든 환경에 적용되는 설정은 `application.rb`파일에 있다. 터미널에서 `bundle rails s thin` 명령어를 통해 레일즈 서버를 실행하면 이 appllication.rb 파일을 읽어들인다.


#### 1.1. 라우팅(Routing) ⭐️

레일즈 라우터는 자바의 `@RequestMapping`와 같이, 인식된 URL 을 통해 컨트롤러의 작업에 보내는 역할을 한다.
`config/routes.rb`에 라우팅 규칙을 설정하면 된다.

**레일즈 가이드 - 루트 경로 연결하기**

```ruby
Blog::Application.routes.draw do
  #...
  # You can have the root of your site routed with "root"
  # just remember to delete public/index.html.
  root :to => "home#index"
end
```
예시 코드는 root 액션을 home 컨트롤러의 index 액션과 연결 된다. (http://localhost:3000/)

##### 1.1.1. URL에서 코드 연결

* `match` : URL을 컨트롤러와 액션에 연결시킨다. - 단일 연결

```
GET /patients/17
```

```ruby
match "/patients/:id" => "patients#show"
```

이 요청은 partients 컨트롤러의 show 액션 에 {:id => 17} 파라미터와 함께 디스패치 된다.
참으로 직관적이다. 

- - -

* `resource` : 리소스에 해당하는 모든 메서드를 컨트롤러에 연결시킨다. 

`:only => [:index, ...]` 을 사용하여 메서드를 한정할 수 있다!

```ruby
resources :subscriptions do
  member do
    get 'unsubscribe'
    # more routes
  end
end

resources :photos do
  collection do
    get 'search'
    # more routes
  end
end
```

member

:id를 낌

collection

안낌

https://medium.com/@tadhao/member-vs-collection-in-rails-routes-ade10c8c5d19

- - -

##### 1.1.2. 코드에서 경로 생성

```ruby
@patient = Patient.find(17)
```
```ruby
<%= link_to "Patient Record", patient_path(@patient) %>
```

위 코드는 `/patients/17` 경로를 생성한다.


##### 1.1.3. CRUD, 요청 메서드와 Action

레일즈에서 resourceful 라우트는 HTTP 요청 메서드와 URL과 컨트롤러 액션 사이의 매핑을 재공한다.
각 동작은 데이터베이스의 CRUD 동작에도 매핑된다. 다음에 대하여

```ruby
resources :photos
```

|HTTP Verb|Path|action|used for|
|---|---|---|---|
|GET|/photos|index|모든 사진을 표시|
|GET|/photos/new|new|새 사진을 생성하기 위한 폼|
|POST|/photos|create|새 사진 생성|
|GET|/photos/:id|show|개별 사진 조회|
|GET|/photos/:id/edit|edit|개별 사진을 수정하는 폼|
|PUT|/photos/:id|update|개별 사진 수정|
|DELETE|/photos/:id|destroy|개별 사진 삭제|

Photos 컨트롤러에 매핑되는 동작들은 위와 같다.

##### 1.1.4. Path와 URLs

resourceful 경로를 사용하면, 컨트롤러에 다음과 같은 helper를 사용할 수 있다.

|helper|return URL|
|---|---|
|photos_path|returns /photos|
|new_photo_path|returns /photos/new|
|edit_photo_path(id)|returns /photos/:id/edit (for instance, edit_photo_path(10) returns /photos/10/edit)|
|photo_path(id)|returns /photos/:id (for instance, photo_path(10) returns /photos/10)|


🔗 <a href="http://rubykr.github.io/rails_guides/routing.html">Rails Routing from the Outside In</a><br>


```
```

```ruby
```

### 2. 어플리케이션(Application)

#### 2.1. 컨트롤러(Controllers)

##### 2.1.1. 랜더링(Rendering)

컨트롤러 관점에서 HTTP 응답을 생성하는 방법 3가지

* `render`를 호출하여 브라우저에 보낼 전체 response 생성
* `redirect_to`호출하여 브라우저에 HTTP 리디렉션 상태코드 전송
*  `head`를 호출하여 브라우저에 HTTP 헤더로만 구성된 response 생성


###### 2.1.1.1. 디폴트 렌더링

<a href="https://stothey0804.github.io/ruby%20on%20rails/rails-1/">ruby on rails 입문</a> 포스트에서 레일즈의 개발철학에는 __Convention Over Configuration(설정보다 관습)__이 있다고 했다. 디폴트 렌더링이 바로 그 예이다.

레일즈 컨트롤러는 유효한 경로에 해당하는 이름으로 뷰를 자동으로 렌더링한다. 

```ruby
class BooksController < ApplicationController
end
```

`BooksController`에 위 코드가 있고, 아래 처럼 라우팅 되어있으며 뷰 폴더 경로에 파일이 존재할 때

**config/routes.rb**
```ruby
resources :books
```

**app/views/books/index.html.erb**
```html
<h1>Books are coming soon!</h1>
```

 `/books` 경로로 접속하면 **Books are coming soon!** 이 출력되는 화면을 볼 수 있다.

이제 실제로 모델을 생성하여 `index`액션에 추가하려면 다음과 같이 코드를 추가한다.

```ruby
class BooksController < ApplicationController
  def index
    @books = Book.all
  end
end
```

index 액션이 끝날때까지 명시적으로 렌더링하지 않으면 레일즈가 컨트롤러의 뷰 경로에서 `action_name.html.erb`를 자동으로 찾아 렌더링 한다.
위 코드의 경우 `/app/views/books/index.html.erb` 파일을 렌더링 하게 된다.


###### 2.1.1.2. 렌더링 사용하기

**렌더링 하지 않기**

```ruby
render :nothing => true
```
```
HTTP/1.1 200 OK
Connection: close
Date: Sun, 24 Jan 2010 09:25:18 GMT
Transfer-Encoding: chunked
Content-Type: */*; charset=utf-8
X-Runtime: 0.014297
Set-Cookie: _blog_session=...snip...; path=/; HttpOnly
Cache-Control: no-cache
```

`/books` 에 대한 응답은 위와 같다.
위와 같이 렌더링 하지 않는 방법은 ajax 요청에 유용할 수 있다고 한다.


**액션 뷰 렌더링**

```ruby
def update
  @book = Book.find(params[:id])
  if @book.update_attributes(params[:book])
    redirect_to(@book)
  else
    render "edit"
  end
end
```

동일 템플릿 내에서 다른 작업에 해당하는 뷰를 렌더링하려면 `render`를 사용한다. 

http://rubykr.github.io/rails_guides/layouts_and_rendering.html

```ruby

```


#### 2.3. 모델(Models) ⭐️

#### 2.4. 뷰(Views)

https://rubykr.github.io/rails_guides/association_basics.html


### 3. 마이그레이션(Migrations)


### 4. 번들러(Bundler)