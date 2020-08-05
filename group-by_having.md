# GROUP BY\_HAVING

## GROUP BY절

* 그룹 기준을 제시할 수 있는 구문
* 해당 그룹기준별로 그룹을 묶어줄 수 있다.
* 여러개의 값들을 하나의 그룹으로 묶어서 처리할 목적으로 사용

#### 

#### ex

```sql
--전체 사원들 총 급여합
SELECT
	SUM( SALARY ) 
FROM
	EMPLOYEE;
--각 부서별 총 급여합
SELECT
	DEPT_CODE,
	SUM( SALARY ) 
FROM
	EMPLOYEE 
GROUP BY
	DEPT_CODE;
```

각 부서별 사원수

```sql
SELECT
	DEPT_CODE,
	COUNT( * ) 
FROM
	EMPLOYEE 
GROUP BY
	DEPT_CODE
```

각 직급별 급여합, 사원수, 보너스를 받는 사원수

```sql
SELECT
	JOB_CODE,
	SUM( SALARY ),
	COUNT( * ),
	COUNT( BONUS ) "보너스 받는 사원수",
	FLOOR( AVG( SALARY ) ) 평균급여,
	MAX( SALARY ) 최고급여,
	MIN( SALARY ) 최저급여 
FROM
	EMPLOYEE 
GROUP BY
	JOB_CODE 
ORDER BY
	1;
```

각 부서별 총 사원수, 보너스를 받는 사원수, 급여합, 평균급여, 최고급여, 최저급여

```sql
SELECT
	DEPT_CODE 부서,
	COUNT( * ) 총사원수,
	COUNT( BONUS ) "보너스 받는 사원수",
	SUM( SALARY ) 급여합,
	ROUND( AVG( SALARY ) ) 평균급여,
	MAX( SALARY ) 최고급여,
	MIN( SALARY ) 최저급여 
FROM
	EMPLOYEE
GROUP BY
	DEPT_CODE;
```



## HAVING

* 그룹에 대한 조건을 제시할 때 사용되는 구문
* 주로 그룹함수한 결과를 가지고 비교수행



#### 각 부서별 평균급여 조회

```sql
SELECT
	DEPT_CODE,
	ROUND( AVG( SALARY ) )
FROM
	EMPLOYEE 
GROUP BY
	DEPT_CODE
HAVING
	AVG( SALARY ) >= 3000000;
```

#### 각 직급별 총 급여합이 1000만원 이상인 직급, 급여합을 조회

```sql
SELECT
	JOB_CODE,
	SUM( SALARY ) 
FROM
	EMPLOYEE 
GROUP BY
	JOB_CODE 
HAVING
	SUM( SALARY ) >= 10000000;
```









#### ROLLUP, CUBE  

* GROUP BY절에 사용되는 함수

ROLLUP - 첫번째 칼럼으로 중간집계

CUBE - 칼럼1과 칼럼2 로 중간집계를 낸다.

각 직급별 급여합

```sql
SELECT
	DEPT_CODE,
	JOB_CODE,
	SUM( SALARY ) 
FROM
	EMPLOYEE 
GROUP BY
	ROLLUP(DEPT_CODE,JOB_CODE) 
ORDER BY
	DEPT_CODE 
	
SELECT
	DEPT_CODE,
	JOB_CODE,
	SUM( SALARY ) 
FROM
	EMPLOYEE 
GROUP BY
	CUBE(DEPT_CODE,JOB_CODE) 
ORDER BY
	DEPT_CODE 
```

### 차이점

{% hint style="warning" %}
그룹기준이 적어도 두개이상의 칼럼이여야 한다.

즉,  CUBE = ROLLUP + a
{% endhint %}



집합연산자

SET OPERATION

* 여러개의 쿼리문을 가지고 하나의 쿼리문으로 만드는 연산자.

종류

* UNION   -   합집합  \(  두 쿼리문 수행한 결과값을 더한 후 중복되는 부분 한번 뺀것\) OR
* INTERSECT    -   교집합 \( 두 쿼리문 수행한 결과값에 중복된 결과값\) AND
* UNION ALL     -    두개의 합쳐진버젼 \(중복된 값이 들어갈 수 있다.\)
* MINUS   -   차집합 \( 선행 쿼리문 결과값 - 후행 쿼리문 결과값\)

#### 급여가 300만원 초과되거나 부서코드가 D5인 사원

```sql
--부서코드가 D5인 사원들만 조회
SELECT
	EMP_ID,
	EMP_NAME,
	DEPT_CODE,
	SALARY 
FROM
	EMPLOYEE 
WHERE
	DEPT_CODE = 'D5'
	
UNION        ---UNION	
--급여가 300만원 초과인 사원들만 조회
SELECT
	EMP_ID,
	EMP_NAME,
	DEPT_CODE,
	SALARY 
FROM
	EMPLOYEE 
WHERE
	SALARY > 3000000
```



