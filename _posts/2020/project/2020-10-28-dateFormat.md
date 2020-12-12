<<<<<<< HEAD
---
title: \[JAVA,SQL] 날짜 비교 출력하기 (방금전, 1시간전 ...)
tags:
- SQL
categories:
- project
---

알림 기능을 구현하고 나니 SNS 작성글 처럼, 알림이 등록된 시간을 현재 시간과 비교하여 표시하고 싶어졌다.

## 기능

* 현재시간과 DB등록시간을 비교하여 등록시간을 방금전, n분전, n시간전, n일전, ... 으로 표시
* 프로젝트를 저장하는 DB는 AWS rds를 사용하므로 DB등록시간이 local서버와 시간이 상이하므로, 기준이 되는 현재시간 또한 DB에서 조회


### Common.java

공통적인 기능을 저장하는 Common 클래스 안에 static 메서드인 `formatTimeString`를 생성한다.
`formatTimeString`은 등록시간을 입력하면 해당하는 결과를 return해준다.

```java
private static class TIME_MAXIMUM {
	public static final int SEC = 60;
	public static final int MIN = 60;
	public static final int HOUR = 24;
	public static final int DAY = 30;
	public static final int MONTH = 12;
}

public static String formatTimeString(String regTime, CommonService commonService) {
	// 비교시간 생성 쿼리
	int diffTime = commonService.selectCompareTime(regTime);
	String msg = null;
	if (diffTime < TIME_MAXIMUM.SEC) {
		msg = "방금 전"; 				// sec
	} else if ((diffTime /= TIME_MAXIMUM.SEC) < TIME_MAXIMUM.MIN) {
		msg = diffTime + "분 전"; 		// min
	} else if ((diffTime /= TIME_MAXIMUM.MIN) < TIME_MAXIMUM.HOUR) {
		msg = (diffTime) + "시간 전"; 	// hour
	} else if ((diffTime /= TIME_MAXIMUM.HOUR) < TIME_MAXIMUM.DAY) {
		msg = (diffTime) + "일 전"; 	// day
	} else if ((diffTime /= TIME_MAXIMUM.DAY) < TIME_MAXIMUM.MONTH) {
		msg = (diffTime) + "달 전"; 	// month
	} else {
		msg = (diffTime) + "년 전"; 	// year
	}
	return msg;
}

```

### Mybatis 설정

```xml
<select id="selectCompareTime" resultType="int" parameterType="String">
	<![CDATA[
		SELECT ((SELECT SYSDATE FROM DUAL) 
		- (SELECT TO_DATE(#{regTime},'YYYY-MM-DD hh24:mi:ss') FROM DUAL ))*24*60*60
		FROM DUAL
	]]>
</select>
```

`selectCompareTime` 메서드의 쿼리는 위와 같다.

현재시간 - 입력받은 시간(컬럼에는 VARCHAR로 저장되어있으므로 TO_DATE를 통해 날짜변환)을 계산하는데,
Oracle DB 시간연산은 일 기준으로 결괏값이 나오기 때문에 \*24\*60\*60 을 곱해야 초 단위까지 비교할 수 있다.

### Controller

아까 만든 `Common.formatTimeString`를 알림 Controller에서 조회 시 사용해준다.

```java
...
// 새로운 알람 List 조회
List<MemberP005VO> newList = memberP005_d001Service.searchNewNotifyList(m_id);
for(MemberP005VO vo : newList) {	// 날짜 포맷 변경
	vo.setN_time(Common.formatTimeString(vo.getN_time(), commonService));
}
...
mav.addObject("newList", newList);
```

MemberP005VO 클래스의 n_time 필드는 YYYY-MM-DD hh24:mi:ss 포맷으로 저장된 String 값이므로
만들어둔 `formatTimeString`를 사용하여 변환시킨 뒤 newList를 view로 보내주면 된다.

=======
---
title: \[JAVA,SQL] 날짜 비교 출력하기 (방금전, 1시간전 ...)
tags:
- SQL
categories:
- project
---

알림 기능을 구현하고 나니 SNS 작성글 처럼, 알림이 등록된 시간을 현재 시간과 비교하여 표시하고 싶어졌다.

## 기능

* 현재시간과 DB등록시간을 비교하여 등록시간을 방금전, n분전, n시간전, n일전, ... 으로 표시
* 프로젝트를 저장하는 DB는 AWS rds를 사용하므로 DB등록시간이 local서버와 시간이 상이하므로, 기준이 되는 현재시간 또한 DB에서 조회


### Common.java

공통적인 기능을 저장하는 Common 클래스 안에 static 메서드인 `formatTimeString`를 생성한다.
`formatTimeString`은 등록시간을 입력하면 해당하는 결과를 return해준다.

```java
private static class TIME_MAXIMUM {
	public static final int SEC = 60;
	public static final int MIN = 60;
	public static final int HOUR = 24;
	public static final int DAY = 30;
	public static final int MONTH = 12;
}

public static String formatTimeString(String regTime, CommonService commonService) {
	// 비교시간 생성 쿼리
	int diffTime = commonService.selectCompareTime(regTime);
	String msg = null;
	if (diffTime < TIME_MAXIMUM.SEC) {
		msg = "방금 전"; 				// sec
	} else if ((diffTime /= TIME_MAXIMUM.SEC) < TIME_MAXIMUM.MIN) {
		msg = diffTime + "분 전"; 		// min
	} else if ((diffTime /= TIME_MAXIMUM.MIN) < TIME_MAXIMUM.HOUR) {
		msg = (diffTime) + "시간 전"; 	// hour
	} else if ((diffTime /= TIME_MAXIMUM.HOUR) < TIME_MAXIMUM.DAY) {
		msg = (diffTime) + "일 전"; 	// day
	} else if ((diffTime /= TIME_MAXIMUM.DAY) < TIME_MAXIMUM.MONTH) {
		msg = (diffTime) + "달 전"; 	// month
	} else {
		msg = (diffTime) + "년 전"; 	// year
	}
	return msg;
}

```

### Mybatis 설정

```xml
<select id="selectCompareTime" resultType="int" parameterType="String">
	<![CDATA[
		SELECT ((SELECT SYSDATE FROM DUAL) 
		- (SELECT TO_DATE(#{regTime},'YYYY-MM-DD hh24:mi:ss') FROM DUAL ))*24*60*60
		FROM DUAL
	]]>
</select>
```

`selectCompareTime` 메서드의 쿼리는 위와 같다.

현재시간 - 입력받은 시간(컬럼에는 VARCHAR로 저장되어있으므로 TO_DATE를 통해 날짜변환)을 계산하는데,
Oracle DB 시간연산은 일 기준으로 결괏값이 나오기 때문에 \*24\*60\*60 을 곱해야 초 단위까지 비교할 수 있다.

### Controller

아까 만든 `Common.formatTimeString`를 알림 Controller에서 조회 시 사용해준다.

```java
...
// 새로운 알람 List 조회
List<MemberP005VO> newList = memberP005_d001Service.searchNewNotifyList(m_id);
for(MemberP005VO vo : newList) {	// 날짜 포맷 변경
	vo.setN_time(Common.formatTimeString(vo.getN_time(), commonService));
}
...
mav.addObject("newList", newList);
```

MemberP005VO 클래스의 n_time 필드는 YYYY-MM-DD hh24:mi:ss 포맷으로 저장된 String 값이므로
만들어둔 `formatTimeString`를 사용하여 변환시킨 뒤 newList를 view로 보내주면 된다.

>>>>>>> a937bcd5bda086a5cff6b5e97a82ecae4ebcc3f8
👍