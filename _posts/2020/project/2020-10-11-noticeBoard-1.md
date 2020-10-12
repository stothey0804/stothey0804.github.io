---
title: \[Spring]ckeditor를 사용한 간단한 공지게시판 만들기-(1)작업계획
tags:
- Spring
categories:
- project
toc: true
---

비전공자 취준생의 입장에서, 모자라지만 스스로 (+구글링을 통해) 터득한 것들을 상세하게 정리해본다.

`#tmi주의`


## 📝 만들어 볼 기능

- 제목을 누르면 내용이 toggel되는 공지사항 게시판
- 입력 폼에 CKEDITOR 적용
- 회원 > 공지사항 조회
- 관리자 > 공지사항 조회, 작성, 수정, 삭제

회원은 사용자 레이아웃에서 공지사항 조회만 가능하며
관리자는 어드민 페이지에서 공지사항을 조회, 작성, 수정, 삭제 한다.

공지사항에 댓글은 허용되지 않고 작성자는 오직 관리자 1명에, 조회 기능에는 검색기능을 넣지 않았다.
(게시판 검색 기능은 다른 게시판에서 구현했으므로 따로 정리해서 올릴 예정!)

*조건이 거의 없어서 쉽고 빨리 만들 수 있고 ... 무엇보다 블로그에 내용정리하기도 간편하다.*😅

- - -

## 작업 진행계획

너무 기본적인 내용이지만 혹시나 정말 시작조차 방법을 모르겠는 분들에게 도움이 될까? 하는 마음으로 진행과정을 정리한다.

### 1. 화면 시안 작성

카카오에서 제공하는 프로토타이핑 툴인 OVEN을 사용하여 간단하게 시안을 작성했다.

>OVEN🔗https://ovenapp.io/

**[참고]** OVEN은 BETA서비스다.
{: .notice}

![어드민-작성폼]({{ site.url }}{{ site.baseurl }}/assets/images/2020-10-11-noticeBoard-1/proto-form.png)
> 글쓰기/수정 화면

![공지사항리스트]({{ site.url }}{{ site.baseurl }}/assets/images/2020-10-11-noticeBoard-1/proto-list.png)
> 공지사항 조회 화면

![공지사항클릭시]({{ site.url }}{{ site.baseurl }}/assets/images/2020-10-11-noticeBoard-1/proto-list2.png)
> 클릭시 토글 화면

어드민 기준으로 만들었지만, 리스트 조회부분은 tiles 레이아웃만 다르지 내용은 같다.

- - -

### 2. DB 엔터티 생성

공지게시판에 저장될 정보는 글번호, 글제목, 내용, 작성일시이며,
모든 데이터의 타입은 VARCHAR2로 설정했고, 식별자는 글번호인 N_ID 컬럼이다.

(수정자, 수정일시는 관리를 위해 모든 테이블에 들어가는 부분이라 skip...)

#### 테이블 구성

![DB컬럼]]({{ site.url }}{{ site.baseurl }}/assets/images/2020-10-11-noticeBoard-1/db-entity.png)
> notice 테이블 구성


#### 테이블 작성 쿼리

```
-- 테이블 생성
CREATE TABLE notice
(
    n_id    VARCHAR2(100) PRIMARY KEY, -- PK
    n_title    VARCHAR2(100) NOT NULL,
    n_content    VARCHAR2(2000) NOT NULL,
    n_date    VARCHAR2(100) NOT NULL,
    n_editor    VARCHAR2(100),
    n_editdate    TIMESTAMP
);
-- 코멘트 생성
COMMENT ON COLUMN notice.n_id IS '글번호';
COMMENT ON COLUMN notice.n_title IS '제목';
COMMENT ON COLUMN notice.n_content IS '내용';
COMMENT ON COLUMN notice.n_date IS '작성일시';
COMMENT ON COLUMN notice.n_editor IS '수정자';
COMMENT ON COLUMN notice.n_editdate IS '수정일자';
COMMENT ON TABLE notice IS '공지사항';
```

- - -

### 3. 개발

#### 개발 순서

1. view layout 작성 및 tiles 연결
2. Controller 메서드 작성
3. VO 및 mybatis mapper 작성
4. DAO, Service 연결
5. view 연결

데이터 연결 전 bootstrap 문서를 참고해 jsp 레이아웃을 미리 만들고 tiles 설정에서 매핑될 viewName을 지정한 후
필요한 기능들을 컨트롤러 인터페이스에 작성하고 메서드에 사용할 쿼리를 mybatis mapper에 만들고
DAO와 Service에 연결시킨 다음 최종적으로 view 에서 전달한 데이터가 출력되도록 한다.

정해진 것도 없는데 뭐 이런거까지 쓰나... 싶지만 지금은 이렇게 하고 있다는 기록을 남기는데 의의를 둔다. 🙄

- - -

게시판 작업계획은 여기까지이며 다음 게시글에서 CKEDITOR 설정방법과 tiles 사용법을 정리할 예정이다!
