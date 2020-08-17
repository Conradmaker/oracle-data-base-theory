# VIEW

## VIEW 

* SELECT 문 \(쿼리문\)을 저장해 둘 수 있는 객체
* 자주쓰는 긴 SELECT문을 저장해두면 긴 SELECT문을 매번 다시 기술할 필요가 없다.

한국에서 근무하는 사원의 사번, 이름, 부서명, 급여, 근무국가명을 조회

```sql
SELECT
	EMP_NO,
	EMP_NAME,
	DEPT_TITLE,
	SALARY,
	NATIONAL_NAME 
FROM
	EMPLOYEE
	JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID )
	JOIN LOCATION ON ( LOCATION_ID = LOCAL_CODE )
	JOIN NATIONAL USING ( NATIONAL_CODE ) 
WHERE
	NATIONAL_NAME = '한국';
```

러시아에서 근무하는 사원의 사번 이름 부서명, 급여, 근무국가명

```sql
SELECT      --여기부터--
	EMP_NO,      
	EMP_NAME,
	DEPT_TITLE,
	SALARY,
	NATIONAL_NAME 
FROM
	EMPLOYEE
	JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID )
	JOIN LOCATION ON ( LOCATION_ID = LOCAL_CODE )
	JOIN NATIONAL USING ( NATIONAL_CODE ) 
	        --여기까지 겹친다.--
WHERE
	NATIONAL_NAME = '러시아';
```

겹치는 부분을 view로 만들어 사용할 수 있음.

## VIEW 생성방법

### 표현법

```sql
CREATE [OR REPLACE] VIEW 뷰이름
AS 서브쿼리;
```

* OR REPLACE : 뷰 생성시 기존에 중복된 뷰가 없다면 새로 뷰를 생성하고,
*                           있다면 해당 뷰를 생신한다.

```sql
CREATE OR REPLACE VIEW VW_EMPLOYEE AS 
(SELECT
	EMP_NO,
	EMP_NAME,
	DEPT_TITLE,
	SALARY,
	NATIONAL_NAME 
FROM
	EMPLOYEE
	JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID )
	JOIN LOCATION ON ( LOCATION_ID = LOCAL_CODE )
	JOIN NATIONAL USING ( NATIONAL_CODE ));
```

#### 한국에서 근무하는 사원

```sql
SELECT
	* 
FROM
	VW_EMPLOYEE --VIEW
WHERE
	NATIONAL_NAME = '한국';
```

### 

### 뷰는 논리적인 가상 테이블 

* 실질적으로 데이터를 저장하고 있지 않다. 
* 단순이 쿼리문이 TEXT문구로 저장되어있는 것이다.

```sql
SELECT
	* 
FROM
	VW_EMPLOYEE;
```

를 호출해보면 SELECT문이 나오게 된다.

또한, 같은 이름의 뷰를 계속 생성하더라도 `OR REPLACE`로 인해 갱신되기 때문에 오류가 나지 않는다.

## 뷰 칼럼에 별칭부여

{% hint style="warning" %}
서브쿼리의 SELECT절에 함수식이나 산술연산식이 기술되어 있는 경우 반드시 별칭을 지정해줘야 한다.
{% endhint %}

```sql
CREATE 
	OR REPLACE VIEW VW_EMP_JOB AS SELECT
	EMP_ID,
	EMP_NAME,
	JOB_NAME,
	DECODE( SUBSTR( EMP_NO, 8, 1 ), '1', '남', '2', '여' ),
	EXTRACT( YEAR FROM SYSDATE ) - EXTRACT( YEAR FROM HIRE_DATE ) "근무년수" 
FROM
	EMPLOYEE
	JOIN JOB USING ( JOB_CODE )
```

다른방법도 있습니다.

```sql
CREATE 
	OR REPLACE VIEW VW_EMP_JOB ( 사번,사원명,직급명,성별,근무년수 ) AS SELECT
	EMP_ID,
	EMP_NAME,
	JOB_NAME,
	DECODE( SUBSTR( EMP_NO, 8, 1 ), '1', '남', '2', '여' ),
	EXTRACT( YEAR FROM SYSDATE ) - EXTRACT( YEAR FROM HIRE_DATE ) 
FROM
	EMPLOYEE
	JOIN JOB USING ( JOB_CODE )
```

단 아래의 방법은 모든 칼럼에 별칭을 부여해줘야만 합니다.

## 주의점

뷰를 사용해 DML 명령어로 조작시 불가능한 경우가 많다.

* 뷰에 정의되어있지 않은 칼럼을 조작하는 경우
* 뷰에 정의되어있지 않은 칼럼중에 베이스테이블상에 NOTNULL제약조건이 있는 경우
* 산술연산식 또는 함수식으로 정의된 경우
* 그룹함수나 GROUP BY절이 포함된경우
* DISTINCT구문이 포함된 경우
* JOIN을 이용해서 여러 테이블을 연결시켜놓은 경우

#### 뷰에 정의되어 있느 않은 칼럼을 조작하는 경우

```sql
	--VIEW 생성
	CREATE OR REPLACE VIEW VW_JOB2 
	AS SELECT JOB_CODE
	FROM JOB
	--INSERT문
	INSERT INTO VW_JOB2(JOB_CODE,JOB_NAME)VALUES('J8','인턴');--오류
	--DELETE
	DELETE FROM VW_JOB2
	WHERE JOB_NAME = '사원' --오류
```

#### 조건에 해당하지 않는다면 베이스테이블에 적용이 될것이다.

```sql
	--VIEW 생성
	CREATE OR REPLACE VIEW VW_JOB3 
	AS SELECT JOB_NAME
	FROM JOB
	
	UPDATE VW_JOB3 SET JOB_NAME='알바' WHERE JOB_NAME = '사원'; 
	 --조건에 해당하지 않으면 베이스 테이블인 JOB에 수정된다.
```

#### 

#### 

#### 산술연산, 함수식이 포함된 경우

```sql
--사원의 사번, 사원명, 급여, 연봉에 대해 조회하는 뷰
	CREATE OR REPLACE VIEW VW_EMP_SAL 
	AS SELECT EMP_ID,EMP_NAME,SALARY,SALARY*12 "연봉"
	FROM EMPLOYEE;
	
	
	--INSERT
	INSERT INTO VW_EMP_SAL VALUES(400,'김모씨',300000,36000000); --연봉이 있어서 문제가 된다. 
	--연봉을 삭제
	INSERT INTO VW_EMP_SAL(EMP_ID,EMP_NAME,SALARY) VALUES(400,'김모씨',300000); 
	
	
	--UPDATE
	UPDATE VW_EMP_SAL SET 연봉=8000000 WHERE EMP_ID=200; 
	--연봉이 있어서 문제가 된다. 
	
	--200번사원의 '급여'를 700으로
	UPDATE VW_EMP_SAL SET SALARY=7000000 WHERE EMP_ID=200;
	--SALARY는 베이스테이블에 있는 칼럼이라 수정가능
```



> VIEW를 가지고 DML을 사용할시 제한조건에 걸리지 않는 경우라면 VIEW로 만든 가상테이블이 아닌 원래의 베이스테이블의 값에 영향이 미친다는 것을 기억하고 있어야 한다.

