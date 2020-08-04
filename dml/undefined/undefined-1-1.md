# 날짜 관련 함수

## 날짜관련 함수

### DATE 타입의 형식

 년/월/일, 시분초

### SYSDATE 

오늘날짜\(시스템날짜\) 반환

```sql
SELECT SYSDATE --2020-08-04 16:56:53
FROM
	DUAL;
```



## MONTHS\_BETWEEN

* 두 날짜 사이의 개월수를 반환

```sql
MONTHS_BETWEEN(DATE1, DATE2) 
```

#### EX

```sql
SELECT
	EMP_NAME,
	FLOOR( SYSDATE - HIRE_DATE ) 근무일수,
	FLOOR(MONTHS_BETWEEN( SYSDATE, HIRE_DATE )) 근무개월수 
FROM
	EMPLOYEE;
```



## ADD\_MONTHS

* 특정 날짜에 해당 숫자만큼의 개월수를 더한 날짜를 반환

```sql
ADD_MONTHS(DATE,NUMBER)
```

```sql
SELECT
	ADD_MONTHS( SYSDATE, 5 )  --오늘날짜로부터 5개월후
FROM
	DUAL;
```



## NEXT\_DAY

* 특정 날짜에서 가장 가까운 해당 요일을 찾아 그 날짜 반환

```sql
NEXT_DAT(DATE, 요일(문자|숫자))
```

```sql
SELECT
	NEXT_DAY( SYSDATE, '목' ) 
	SELECT
	NEXT_DAY( SYSDATE, '목요일' ) 
	SELECT
	NEXT_DAY( SYSDATE, 5 ) --1:일 2:월 3:화 ....
FROM
	DUAL;
```

### 언어변경

```sql
ALTER SESSION SET NLS_LANGUAGE = AMERICAN; --영어로 변경
```





## LAST\_DAY

* 특정 월의 마지막 날짜를 찾아 그 날짜 반환

```sql
SELECT LAST_DAY(SYSDATE) FROM DUAL
```

```sql
SELECT
	EMP_NAME,
	HIRE_DATE,
	LAST_DAY( HIRE_DATE ) 
FROM
	EMPLOYEE;
```





## EXTRACT

* 년도, 월, 일 정보를 추출해서 반환 

{% hint style="warning" %}
NUMBER 타입 반환
{% endhint %}

```sql
EXTRACT(YEAR FROM DATE) --특정 날짜로부터 년도만 추출
EXTRACT(MONTH FROM DATE) --특정 날짜로부터 월만 추출
EXTRACT(DAY FROM DATE) --특정 날짜로부터 일만 추출
```

```sql
SELECT
	EMP_NAME EXTRACT( YEAR FROM HIRE_DATE ) 입사년도,
	EXTRACT( MONTH FROM HIRE_DATE ) 입사월,
	EXTRACT( DAY FROM HIRE_DATE ) 입사일 
FROM
	EMPLOYEE;
```



