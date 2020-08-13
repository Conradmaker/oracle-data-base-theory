# INSERT

## INSERT

테이블에 새로운 행을 추가하는 구문

### 표현법

#### 

#### 테이블에 모던 칼럼에 대한 값을 한행 INSERT할때

```sql
INSERT INTO 테이블명 VALUES(값,값,값,...);
```

{% hint style="warning" %}
칼럼순번에 맞춰서 값 나열 / 존재하는 칼럼수만큼 값 제시
{% endhint %}

```sql
INSERT INTO EMPLOYEE
VALUES
	( 900, '장채현', '980914-2112457', 
	'jang_ch@gmail.com', '010-9999-9999',
	'D1''J8', 2000000, NULL, 200, SYSDATE, NULL, DEFAULT );
```

#### 

#### 선택한 칼럼에만 넣을때

```sql
INSERT INTO 테이블명(칼럼명, 칼럼명, 칼럼명) VALUES(값,값,값);
```

{% hint style="warning" %}
한 행단위로 차가되기 때문에 지정하지 않은 칼럼에는 NULL값이 들어간다.
{% endhint %}

#### 

#### 서브쿼리수행결과INSERT

### INSERT INTO

* 한개의 테이블에 인서트할때

```sql
FROM
	EMPLOYEE
	LEFT JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID );
INSERT INTO EMP_01 ( SELECT EMP_ID, EMP_NAME, DEPT_TITLE FROM EMPLOYEE LEFT JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID ); )
```



### INSERT ALL

* 서브쿼리결과중에 해당 칼럼만 뽑아서 추가

```sql
INSERT ALL
INTO 테이블명1 VALUES(컬럼명,컬럼명,컬럼명...)  --서브쿼리한 결과중에 칼럼
INTO 테이블명2 VALUES(칼럼명,칼럼명,칼럼명...)  --서브쿼리한 결과중에 칼럼
    서브쿼리;
```



