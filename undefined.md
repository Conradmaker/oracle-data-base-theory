# 그룹함수

## 

## SUM

* 총 합을 결과로 리턴

#### 전 사원의 총 급여합

```sql
SELECT SUM(SALARY)
FROM EMPLOYEE;
```

#### 남자사원 총 급여 합

```sql
SELECT
	SUM( SALARY ) 
FROM
	EMPLOYEE 
WHERE
	SUBSTR( EMP_NO, 8, 1 ) = '1'
```



## AVG

* 해당 칼럼값들의 평균값을 구해서 반환

#### 전사원의 평균급여

```sql
WHERE
	SUBSTR( EMP_NO, 8, 1 ) = '1' SELECT
	AVG( SALARY ) 
FROM
	EMPLOYEE
```



## MIN / MAX

* 해당 칼럼값들 중에 가장 작은 / 큰 값 반환

#### 전 사원들중 가장 \(작은/큰\) 급여

```sql
SELECT
	MIN( SALARY ) 
FROM
	EMPLOYEE
	
SELECT
	MAX( SALARY ) 
FROM
	EMPLOYEE
```





## COUNT 

* 행 갯수를 세서 반환

```sql
COUNT(*|칼럼명|DISTINCT 칼럼명|)
```

| COUNT\(  \) |  |
| :--- | :--- |
| \(\*\) | 결과에 해당하는 모든 행 갯수 |
| \(칼럼명\) | 칼럼의 NULL을 제외한 모든 칼럼행 갯수  |
| \(DISTINCT 칼럼명\) | 칼럼값이 중복일경우 하나로 취급 |

#### 

#### 부서배치가 된 사원\(DEPT\_CODE\)값이 있는 사원

```sql
SELECT
	COUNT( DEPT_CODE ) 
FROM
	EMPLOYEE;
```

#### 사원들이 속해있는 부서의 수

```sql
SELECT
	COUNT( DISTINCT DEPT_CODE ) 
FROM
	EMPLOYEE
```



