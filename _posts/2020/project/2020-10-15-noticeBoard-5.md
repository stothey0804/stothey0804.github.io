---
title: \[Spring] ckeditor를 사용한 간단한 공지게시판 만들기 - (5)페이징
tags:
- Spring, Pagination
categories:
- project
toc: true
---

## 시작하기 전에...

페이징 클래스는 책을 봐도 있고 검색해보면 잘 정리해주신 분이 많다. 이전에 실습을하며 작성했으나 따로 백업을 하지 않아서, (백업이 이렇게 중요하다.)
잘 정리된 블로그를 참조하여 프로젝트에 적용시켰다.

참고 블로그🔗
<a href="https://gangnam-americano.tistory.com/18">https://gangnam-americano.tistory.com/18</a>
> Dev.Project / [spring/게시판] #6 게시판 페이징 처리하기

(정말 감사합니다.)

링크에 있는 Pagination 객체를 사용해 실제 프로젝트에 적용하는 과정을 기록하는 점 참고 바란다.


## 1. 총 페이지 수 구하기

### QUERY

```html

<!-- 전체 게시글 수 조회 -->
<select id="selectNoticeListCnt" resultType="int">
<![CDATA[
    SELECT COUNT(n_id)
    FROM notice
]]>
</select>

```

notice 게시판은 검색조건이 없어 총 게시글 조회 쿼리가 엄청나게 간단하다. COUNT(PK)로 조회하면 끝이다.


### Controller

#### 1. `listCnt` 값 할당

위에서 작성한 selectNoticeListCnt 쿼리를 DAO의 selectNoticeListCnt 메서드에 연결한 뒤,
컨트롤러의 조회 메서드 안에서 실행시킨다.

```java
int listCnt = rootP003_d001Service.selectNoticeListCnt();	// 전체 게시글 수 조회
```

이렇게 저장된 `listCnt` 변수는 Pagination 객체 생성시 사용한다.


#### 2. `curPage` 값 할당

`curPage`는 view에서 페이지 번호를 클릭했을 때 들어오는 값이다.
RequestParam 애너테이션을 통해 받아주면 된다.

```java

// page 값이 없을 시 디폴트 값은 1로 설정한다.
public ModelAndView searchNotice(@RequestParam(defaultValue = "1") int curPage, HttpServletRequest request) throws Exception {
	...
}

```

## 2. Pagination 객체 생성

글 최상단에 있는 링크를 참조하여 Pagination Class를 만들었다면
위에 할당한 `listCnt`와 `curPage`를 사용하여 Pagination 인스턴스를 만들어준다.

```java
public ModelAndView searchNotice(@RequestParam(defaultValue = "1") int curPage, HttpServletRequest request) throws Exception {
	...
    Pagination pagination = new Pagination(listCnt, curPage);	// pagination 객체 생성
}
```

## 3. startIndex, EndIndex 설정

### QUERY

다음은 공지사항 리스트를 조회하는 쿼리다.

```html
<!-- 공지 조회 -->
<select id="selectNoticeList" resultMap="p003Result" parameterType="java.util.HashMap">
    <![CDATA[
        SELECT *
        FROM (
        SELECT n_id, n_title, n_content, REPLACE(n_date,'/','-')n_date,
        ROW_NUMBER() OVER (ORDER BY n_id DESC) ROWNUMBER
        FROM notice)
        WHERE ROWNUMBER BETWEEN #{startIndex} AND #{endIndex}
        ORDER BY n_id desc
    ]]>
</select>
```

resultMap은 notice 테이블과 대응하는 vo이며, 파라미터 타입은 해시맵이다.
페이징 하기전에는 (검색 조건이 없는 게시판이기 때문에) 파라미터가 없어도 됐으나,
페이징을 하게되면 시작 인덱스 - 끝 인덱스 기준으로 조회해야하므로 startIndex와 endIndex를 받아야한다.


### Controller

```java
// 조회용 Parameter 생성
Map<String, String> searchParam = new HashMap<String, String>();
searchParam.put("startIndex", (pagination.getStartIndex()+1)+"");	// 시작 index는 1부터 이므로 1을 더해줌.
searchParam.put("endIndex", (pagination.getStartIndex()+pagination.getPageSize())+"");	// 끝 index
// Notice List 조회
List<RootP003VO> list = rootP003_d001Service.searchNoticeList(searchParam);
```

startIndex와 endIndex의 값은 생성한 `pagination` 인스턴스로 얻어올 수 있는데,
기존 클래스의 모양을 안건드리고 사용하다보니 값을 얻어오는 방법이 조금 거슬린다... 


## 4. View 설정

### Controller

```java
mav.addObject("pagination", pagination);	//페이지네이션
mav.addObject("articleList",list);			//글목록
```

그렇게 생성한 `pagination`과 `list`는 ModelAndView 인스턴스에 addObject() 하여
view로 전달해준다.

### View

articleList 관련 태그는 생략하고 Pagination 부분만 첨부한다.
부트스트랩 문서에 Pagination 항목을 참고하여 작성했다.

부트스트랩🔗 https://getbootstrap.com/docs/4.5/components/pagination/


```html
<!-- 페이징  -->
<nav aria-label="Page navigation example">
    <ul class="pagination justify-content-center">
        <c:if test="${pagination.curRange ne 1 }">
            <li class="page-item">
            <a class="page-link" href="?curPage=1">처음</a> 
            </li>
        </c:if>
        <c:if test="${pagination.curPage ne 1}">
            <li class="page-item">
                <a class="page-link" href="?curPage=${pagination.prevPage}">이전</a> 
                </li>
        </c:if>
        <c:forEach var="pageNum" begin="${pagination.startPage}" end="${pagination.endPage}">
            <c:choose>
                <c:when test="${pageNum eq  pagination.curPage}">
                    <li class="page-item active">
                        <a class="page-link" href="?curPage=${pageNum}">${pageNum }</a>
                    </li>
                </c:when>
                <c:otherwise>
                    <li class="page-item">
                        <a class="page-link" href="?curPage=${pageNum}">${pageNum }</a>
                    </li>
                </c:otherwise>
            </c:choose>
        </c:forEach>
        <c:if test="${pagination.curPage ne pagination.pageCnt && pagination.pageCnt > 0}">
            <li class="page-item">
                <a class="page-link" href="?curPage=${pagination.nextPage}">다음</a> 
                </li>	                        
        </c:if>
        <c:if test="${pagination.curRange ne pagination.rangeCnt && pagination.rangeCnt > 0}">
            <li class="page-item">
                <a class="page-link" href="#"  onClick="fn_paging('${pagination.pageCnt }')">끝</a> 
                </li>	                    
    </c:if>
    </ul>
</nav>
<!-- 페이징 END -->
```

## 결과 확인

![결과화면]({{ site.url }}{{ site.baseurl }}/assets/images/2020-10-11-noticeBoard-5/result.PNG)
> 결과 화면

잘 적용된다.