---
title: \[rails] 레일즈 모델 - 데이터 추출하기
categories:
- Ruby on rails
tags:
- ruby
toc: true
---

실무 로직을 짜보니 레일즈가 익숙하지 않아 데이터에 접근할 때부터 매우... 불편하다.
나름대로 가이드도 보고 공부했다고 생각했는데 역시 제대로 정리하지 않으면 부질 없다. 
가장 기본이 되는 데이터 추출 방법에 대해 정리해보도록 한다.

> 요약 내용은 **퍼펙트 루비 온 레일즈(Perfect Ruby on Rails 4, 야마다 요시히로 저)**를 참고했습니다.

## 목표

- 데이터 추출에 기본이 되는 find 메서드를 알아본다.
- 여러 조건이 필요할때 사용하는 쿼리 메서드를 알아본다.
- 데이터 추출을 위한 추가 메서드들을 알아본다.

## 1. 데이터 추출 기본 - find

### 1.1. 주 키로 검색

```
find(keys)
```
- keys : 주 키 (배열로 지정 가능)

PK인 id로 검색할때 사용한다고 보면 된다.
단일 id로 검색할 수도 있고, 배열을 통해 복수의 id를 검색할 수도 있다!
다음 예시코드는 books 테이블에서 id가 2, 5, 10인 값을 추출한다.

```ruby
def find
  @books = Book.find([2, 5, 10])
  render 'hello/list'
end
```

```sql
SELECT "books".* FROM "books" WHERE "books"."id" IN (2, 5, 10)
```


### 1.2. 임의의 필드 검색

`find_by`는 레일즈 **4.0** 부터 추가된 메서드다. 
`find_by`를 사용하면 임의의 필드를 검색할 수 있으며, 가장 처음 검색되는 레코드를 추출한다. (LIMIT 1)

```
find_by(key: value[, ...])
```
- key : 검색하고자 하는 필드명
- value : 검색 값

```ruby
# 출판사가 jpub이고 가격이 26000원인 도서 검색
def find_by
  @book = Book.find_by(publish: 'jpub', price: 26000)
  render 'books/show'
end
```

```sql
SELECT "books".* FROM "books" WHERE "books"."publish" = 'jpub' AND "books"."price" = 26000 LIMIT 1
```

## 2. 복잡한 조건 검색 - 쿼리 메서드

### 주요 메서드 표

레일즈 4에는 데이터 추출을 위한 다음의 메서드들이 있다.

|메서드|설명|
|---|---|
|where|조건 필터링|
|not|부정 조건식을 표현|
|order|정렬|
|reorder|재정렬|
|select|레코드 지정|
|distinct|중복 제거|
|limit|추출 레코드 수 지정|
|offset|추출 시작 위치 지정|
|group|특정 키로 결과를 그룹화|
|having|GROUP BY에 추가 조건 부여|
|joins|다른 테이블과 결합|
|includes|관련된 모델을 한꺼번에 추출|
|readonly|추출한 객체를 읽기 전용으로 변경|
|none|아무것도 없는 결과 집합을 리턴|

쿼리 메서드는 `find` 또는 `find_by` 등의 메서드와 다르게 호출 시점에는 데이터베이스에 접근하지 않고, 조건식이 들어간 결과를 `ActiveRecord::Relation`객체로 리턴하며 이후 필요한 시점에 접근하게 된다 - 이 것을 **Lazy Load** 라고 한다!

따라서, **메서드 체인**을 사용할 수 있다. 

각 메서드의 사용법에 대해 정리할 것인데, 블로그에서는 4.0 이상의 메서드는 나중에 정리하기로 한다.

### 2.1. where

#### 2.1.1. 해시를 사용한 where 조건식

`필드이름: 값` 형태로 데이터를 찾는다.

```ruby
def where
  @books = Book.where(publish: 'jpub')
  render 'hello/list'
end
```
```sql
SELECT "books".* FROM "books" WHERE "books"."publish" = 'jpub'
```

또한 다음과 같이 `AND`,`BETWEEN`, `IN` 등 조건을 지정할 수 있다

```ruby
# WHERE "books"."publish" = 'jpub' AND "books"."price" = 33600
@books = Book.where(publish: 'jpub', price: 33600)  
# WHERE "books"."published" BETWEEN '2013-06-01' AND '2013-12-31'
@books = Book.where(published: '2013-06-01'..'2013-12-31')  
# WHERE "books"."publish" IN ('jpub', 'hanbit')
@books = Book.where(publish: ['jpub','hanbit']) 
```

#### 2.1.2. 플레이스 홀더를 사용한 조건식

해시를 사용하면 간단하지만 표현범위가 제한적이다.
실무에서도 플레이스 홀더를 사용하는 방식으로 데이터를 조회하고 있었다.

`where(exp [,value, ...]`
- exp : 조건식(플레이스홀더 포함)
- value :  플레이스 홀더에 전달할 값

**플레이스 홀더(Placeholder)**란, 매개 변수를 두는 장소다.

**View**
```ruby
<%= form_tag action: :ph1 do %>
  <div class="field">
    <%= label_tag :publish, '출판사' %>
    <%= text_field_tag :publish %>
  </div>
  <div class="field">
    <%= label_tag :price, '최저가' %>
    <%= text_field_tag :price %>
  </div>
    <%= submit_tag '검색' %>
<% end %>
```

**Controller**
```ruby
def ph1
  @books = Book.where('publish = ? AND price >= ?', params[:publish], parmas[:price])
  render 'hello/list'
end
```

`?`기호가 플레이스홀더 이며, 이후 매개변수의 값을 통해 전달한다. 스프링에서도 써본 방식이다.
플레이스홀더를 사용하면 조건식과 매개변수를 분리할 수 있어서 조건식이 복잡하더라도 쉽게 살펴볼 수 있다.

### 2.2. order

데이터 정렬 메서드이다.

```ruby
...
  # SELECT * FROM books WHERE publish = 'jpub' ORDER BY published DESC
  @books = Book.where(publish: 'jpub').order(published: :desc)
  @books = Book.where(publish: 'jpub').order('published desc')
```

### 2.3. reorder

`order` 메서드 체인을 구성해서 사용할 때, 이전에 작성한 정렬식을 무시하고 새롭게 정렬하거나,
정렬식 자체를 제거하고 싶을 떄 `reorder`를 사용한다.

```ruby
# SELECT * FROM books ORDER BY price ASC
@books = Book.order(:publish).reorder(:price)

# 조건식 제거
# SELECT * FROM books
@books = Book.order(:publish).reorder(nil)
```

### 2.4. select

추출할 필드를 명시적으로 지정한다. 

```ruby
# SELECT title, price FROM books WHERE price >= 20000
@books = book.where('price >= 20000').select(:title, :price)
```

`select`메서드로 추출한 필드 이외의 필드에 접근하면 `ActiveRecord::MissingAttributeError` 에외가 발생하는 점에 유의한다.

### 2.5. distinct

중복되지 않는 레코드를 추출한다.
SELECT 명령어에 DISTINCT 구문을 추가해서 사용하는 것과 같은 효과를 낸다.

```ruby
#SELECT DISTINCT publish FROM books ORDER BY publish ASC
@books = Book.select(:publish).distinct.order(:publish)
```

### 2.6. limit, offset

지정 범위의 레코드를 추출할 때 사용한다.

`limit(rows)`
`offset(off)`
- rows : 최대 추출 레코드 수
- off : 추출 시작 위치(첫번째 레코드는 0)

```ruby
# 출간일(published)로 정렬하고, 5~7번째 데이터를 추출하는 코드
@books = Book.order(published: :desc).limit(3).offset(4)
```

`limit`와 `offset`을 사용할 떄는 `order`메서드를 사용해야 의미가 있으며, 페이징 처리를 구현할 수 있다. (236p 참조)

### 2.7. first, last

결과 집합의 첫번째와 마지막 레코드를 추출한다. `limit`를 사용해도 되나 직관적으로 보기 위해 `fisrt`와 `last`를 사용한다.

```ruby
# 가장 최근에 출간된 레코드 조회
@books = Book.order(published: :desc).last
```

`fisrt`와 `last`는 체인의 맨 마지막에 사용할 것을 명심하자.


### 2.8. group

특정 키로 데이터를 그룹화하고 싶은 경우 사용한다.

```ruby
# 출판사별 가격 평균을 보여주는 코드
def groupby
  @books = Book.select('publish, AVG(price) AS avg_price').group(:publish)
end

```
### 2.9. having

그룹화한 결과에 추가 조건을 적용할 수 있다.

```ruby
# 평균 가격이 25000원 이상인 출판사 정보만
def havingby
  @books = Book.select('publish, AVG(price) AS avg_price').group(:publish).having('avg_price >= ?', 25000)
end
```

## 3. 데이터 추출을 위한 추가 메서드

추가로 알아두면 편리한 메서드를 살펴본다.

### 3.1. 존재 확인 - exists?

데이터를 추출하지 않고 지정된 데이터가 있는지 정도만 확인할 때 사용한다.

```ruby
def exist
  flag = Book.where(publish: '프리렉').exist?
  render text: "존재하나요? : #{flag}"
end
# SELECT 1 AS one FROM books WHERE publish = '프리렉' LIMIT 1
```

`where`와 사용하지 않고 단독으로 사용하는 것도 가능하다.

```ruby
# id가 1인 레코드 존재여부
Book.exists?(1)
# price 필드가 5000이상인 레코드 존재 여부
Book.exists?(['price > ?', 5000])
# publish 필드가 'jpub'인 레코드 존재여부
Book.exists?(publish: 'jpub')
# 테이블에 데이터가 존재하는지 여부 (유용하게 쓰인다!)
Book.exists?
```

### 3.2. 이름있는 스코프(Named Scope)

이름있는 스코프는 자주 사용하는 조건문을 미리 지정하는 것이라고 보면 된다.

이름있는 `scope`는 모델 클래스에서 정의할 수 있다.

```ruby
Class Book < ActiveRecord::Base
  scope :jpub, -> { where(publish: 'jpub') }
  scope :newer, -> { order(published: :desc) }
  scope :top10, -> { newer.limit(10) }
end
```

위 코드는 각각

- publish 필드가 jpub인 도서만 추출하는 jpub 스코프
- published(출간일자)가 최신순인 순서로 정렬하여 추출 newer 스코프
- 출간일자가 최신인 순서로 10개만 추출 top10 스코프

를 정의하는 코드이다.

정의된 스코프는 메서드 체인처럼 연결해서 사용할 수 있다.

```ruby
@books = Book.jpub.top10
# SELECT * FROM books WHERE publish = 'jpub' ORDER BY published DESC LIMIT 10
```

또한 Named Scope 에 매개변수도 사용할 수 있다.

```ruby
Class Book < ActiveRecord::Base
  ...
  scope :whats_new, ->(pub) {
    where(publish: pub).order(published: :desc).limit(5)
  }
end
```

### 3.3. default_scope

기본 스코프는, 모델 관련 메서드를 호출할 때 자동으로 조건이 적용되게 하는 스코프이다.
예를 들어 review 테이블의 내용을 항상 수정일자 순으로 정렬해서 추출할 경우 `default_scope`를 만들면 코드의 반복을 줄일 수 있다.

**Model**
```ruby
Class Review < ActiveRecord::Base
  ...
  default_scope { order(updated_at: :desc) }
end
```
**Controller**
```ruby
def def_scope
  render text: Review.all.inspect
end
# SELECT * FROM reviews ORDER BY update_at DESC
```

기본 스코프를 해제할 때는 `unscoped` 메서드를 사용해야함에 주의하자.

### 3.4. count

추출한 레코드의 개수를 확인할 수 있다. (유용하다.)

```ruby
def count
  cnt = Book.where(publish: 'jpub').count
  render text: "#{cnt}개 입니다."
end
```

where과 함께 사용하지 않아도 사용할 수 있다.

```ruby
# 테이블의 전체 레코드 수
Book.count
# publish 필드가 비어있지 않은 레코드 수
Book.count(:publish)
# publish 필드의 종류 수 (출판사 개수)
Book.distinct.count(:publish)
```

### 3.5. 기타 연산메서드

|메서드|설명|
|---|---|
|average(col)|평균 값|
|minimum(col)|최솟값|
|maximum(col)|최대값|
|sum(col)|합계 값|

### 3.6. sql 직접지정 - find_by_sql

대부분 쿼리 메서드로 처리할 수 있으나 아주 복잡한 질의 일 경우 SQL 명령어 자체를 날리는게 나을 때가 있다.
그럴 때 사용하는 것이 `find_by_sql`이다.

```ruby
def literal_sql
  @books = Book.find_by_sql(['SELECT publish, AVG(price) AS avg)price 
    FROM "books" GROUP BY publish HAVING AVG(price) >= ?', 25000])
end
```

`find_by_sql` 메서드에는 `['sql query', value, ...]` 과 같은 배열형식으로 작성하는 것이 기본이다. (플레이스홀더가 포함되어있지 않으면 단일 문자열로 작성해도 된다.)