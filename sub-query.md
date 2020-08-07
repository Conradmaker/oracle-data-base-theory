# SUB QUERY

## SUB QUERY

* 하나의 SQL문\(INSERT,CREATE...\) 안에 포함된 또다른 SELECT문
* 메인 SQL문을 위해 보조 역할을 하는 쿼리문

만약 SUBQUERY없이 노옹철사원과 같은 부서의 사원들을 검색하기 위해서는 다음과 같다.

```sql
SELECT
	DEPT_CODE 
FROM
	EMPLOYEE E 
WHERE
	E.EMP_NAME = '노옹철';
	
SELECT
	EMP_NAME 
FROM
	EMPLOYEE 
WHERE
	DEPT_CODE = 'D9';
```

2번을 조회해야만 했다.

SUB QUERY를 사용한다면

```sql
SELECT
	EMP_NAME 
FROM
	EMPLOYEE 
WHERE
	DEPT_CODE = ( SELECT DEPT_CODE 
					  FROM EMPLOYEE
					  WHERE EMP_NAME = '노옹철' );
```

이렇게 구현할 수 있다.

#### 전 직원의 평균 급여 이상인 사원들

1. 평균급여를 구한뒤
2. 평균급여보다 높은 직원을 조회한다.

```sql
SELECT
	EMP_ID,
	EMP_NAME,
	JOB_CODE,
	SALARY 
FROM
	EMPLOYEE 
WHERE
	SALARY >= ( SELECT AVG( SALARY ) FROM EMPLOYEE )
```

## 서브쿼리의 구분

서브쿼리를 수행한 결과값이 몇행 몇열이냐에 따라서 분유됨



### 단일행 \[단일열\] 서브쿼리

* 결과값이 오직 1개뿐일때
* 위의 예제들

### 다중행 \[단일열\] 서브쿼리

* 결과값의 행수가 여러행일때



## 단일행 서브쿼리\(Single Row SubQeury\)

서브쿼리의 조회 결과값 갯수가 오로지 1개일 때

일반연산자 사용가능

```sql
SELECT
	EMP_NAME,
	JOB_CODE,
	SALARY 
FROM
	EMPLOYEE 
WHERE
	SALARY < ( SELECT AVG( SALARY ) FROM EMPLOYEE );
	           --서브쿼리 결과값 1열 1행
```

최저급여를 받는 사원의 사번, 이름, 직급코드, 급여, 입사일 조회

```sql
SELECT
	EMP_NAME,
	JOB_CODE,
	SALARY,
	HIRE_DATE 
FROM
	EMPLOYEE 
WHERE
	SALARY = ( SELECT MIN( SALARY ) FROM EMPLOYEE );
```

노옹철 사원의 급여보다 더 많이 받는 사원들의 사번, 이름, 부서코드, 직급코드, 급여

```sql
SELECT
	EMPID,
	EMP_NAME,
	DEPT_CODE,
	JOB_CODE,
	SALARY,
	DEPT_TITLE 
FROM
	EMPLOYEE
	JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID ) 
WHERE
	SALARY > (SELECT SALARY 
						FROM EMPLOYEE 
						WHERE EMP_NAME = '노옹철' )
```

부서별 급여합이 가장 큰 부서만을 조회 부서코드, 급여합 조회

```sql
SELECT
	DEPT_CODE,
	SUM( SALARY ) 
FROM
	EMPLOYEE 
GROUP BY
	DEPT_CODE 
HAVING
	SUM( SALARY ) = ( SELECT MAX( SUM( SALARY ) ) 							
										FROM EMPLOYEE 							 
										GROUP BY DEPT_CODE );
```

## 다중행 서브쿼리\(Multi row SubQuery\)

* 서브쿼리의 조회 결과값이 여러행일 때
* IN 서브쿼리 / NOT IN 서브쿼리 : 여러개의 결과값 중에서 한개라도 일치하는 값이 있으면 / 없다면 이라는 의미
* &gt;ANY 서브쿼리 : 여러개의 결과값중에서 '하나라도' 클경우 /여러개의 결과값 중에서 가장 작은값보다 클경우
* &lt;ANY 서브쿼리 : 여러개의 결과값중에서 '하나라도' 작을경우 /여러개의 결과값 중에서 가장 큰값보다 작을경우
* &gt;ALL 서브쿼리 : 여러개의 결과값중 모든 값보다 클경우/ 여러개의 결과값중에서 가장 큰값보다 클경우
* &lt;ALL 서브쿼리 : 여러개의 결과값중 모든 값보다 작을경우/ 여러개의 결과값중에서 가장 작은값보다 작을경우

### IN 서브쿼리

부서코드 D5또는 D7인 사원들을 조회

```sql
SELECT
	* 
FROM
	EMPLOYEE 
WHERE
	DEPT_CODE IN ( 'D5', 'D6', 'D7' );
```

각 부서별 최고급여

```sql
SELECT
	EMP_NAME,
	JOB_CODE,
	DEPT_CODE,
	SALARY 
FROM
	EMPLOYEE 
WHERE
	SALARY IN ( SELECT MAX( SALARY ) 
					FROM EMPLOYEE 
					GROUP BY DEPT_CODE );
```



### ANY 서브쿼리

사원 &gt; 대리 &gt; 과장 &gt; 차장 &gt; 부장..

대리직급이지만 과장의 최소급여보다 많이받는 직원 조회

```sql
SELECT
	EMP_ID,
	EMP_NAME,
	JOB_NAME,
	SALARY 
FROM
	EMPLOYEE
	JOIN JOB USING ( JOB_CODE ) 
WHERE
	JOB_NAME = '대리' 
	AND SALARY > ANY ( SELECT SALARY 
										 FROM EMPLOYEE 
										 JOIN JOB USING ( JOB_CODE ) 
										 WHERE JOB_NAME = '과장' );
```



## \[단일행\] 다중열 서브쿼리

* 조회결과 값은 하나지만 여러 열\(칼럼\)이 있는것

ex

하이유 사원과 같은 부서코드, 같은 직급코드에 해당하는 사원들

```sql
SELECT
	EMP_NAME,
	DEPT_CODE,
	JOB_CODE,
	HIRE_DATE 
FROM
	EMPLOYEE 
WHERE
	( DEPT_CODE, JOB_CODE ) = ( SELECT DEPT_CODE, JOB_CODE 
															FROM EMPLOYEE 
															WHERE EMP_NAME='하이유' )
```

```sql
SELECT
	EMP_ID,
	EMP_NAME,
	JOB_CODE,
	MANAGER_ID 
FROM
	EMPLOYEE 
WHERE
	( JOB_CODE, MANAGER_ID ) = ( SELECT JOB_CODE, MANAGER_ID 
															 FROM EMPLOYEE 
															 WHERE EMP_NAME = '박나라' )
```



## 인라인뷰

### TOP-N 분석 

상위 몇명 조회할때

전 직원중 급여가 가장 높은 상위 5명

* ROWNUM: 조회된 순서대로 1부터 순번을 부여해주는 칼럼

```sql
SELECT 
   ROWNUM,
	EMP_NAME,
	SALARY 
FROM
	( SELECT EMP_NAME, SALARY, DEPT_CODE FROM EMPLOYEE ORDER BY SALARY  DESC) 
WHERE
	ROWNUM < 6;
```

각 부서별 평균급여가 높은 3개의 부서의 부서코드, 평균급여

```sql
SELECT 
	DEPT_CODE,
	ROUND(E)
FROM
	( SELECT DEPT_CODE, AVG( SALARY ) E FROM EMPLOYEE GROUP BY DEPT_CODE ORDER BY 2 DESC ) 
WHERE
	ROWNUM < 4;
```



순위 매기는 함수

RANK\(\) OVER\(정렬기준\)  /  DENSE\_RANK\(\) OVER\(정렬기준\)

* RANK\(\) OVER: 공동 1위가 2명이면 그 다음은 3위
* DENSE\_RANK\(\) OVER: 공동 1위가 10명이여도 그다음은 2위

