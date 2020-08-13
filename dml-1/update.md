# UPDATE

## UPDATE

태이블에 기록도어 있는 데이터를 수정하는 구문

### 기본 표현법

```sql
UPDATE 테이블명
SET 칼럼명 = 바꿀값,
    칼럼명 = 바꿀값, ... -->여러개의 칼럼값 동시에 변경가능 (,로나열!! AND아님)
    [WHERE] -->생략하면 전체 모든 행의 데이터가 변경
```

#### DEPT\_ID가 D9인 부서명이 '전략기획팀'으로 수정

```sql
UPDATE DEPT_CODE 
SET DEPT_TITLE = '전략기획팀' 
WHERE
	DEPT_ID = 'D9';
```



#### 모든 사원의 급여를 기존의 급여에 10프로 인상한 금액으로 변경

```sql
UPDATE EMP_SALARY 
SET SALARY = SALARY * 1.1;
```

### 

### UPDATE시 SUBQUERY사용

즉, 서브쿼리로 조회한 결과같으로 변경하겠다 라는 의미이다.

```sql
UPDATE 테이블명
    SET 칼럼명 = (서브쿼리)
    [WHERE]
```

#### 

#### 방명수 사원의 급여와 보너스값을 유재식 사원의 급여와 보너스 값으로 변경;

```sql
UPDATE EMP_SALARY 
SET SALARY = ( SELECT SALARY 
					FROM EMP_SALARY 	
					WHERE EMP_NAME = '유재식' ),
	 BONUS = ( SELECT BONUS 
				  FROM EMP_SALARY 	
			     WHERE EMP_NAME = '유재식' )
```

이걸 다중열 서브쿼리로 만들수 있다.

```sql
UPDATE EMP_SALARY 
SET ( SALARY, BONUS ) = ( SELECT SALARY, BONUS 
								  FROM EMP_SALARY 	
								  WHERE EMP_NAME = '유재식' )
```

#### 노옹철, 전형돈, 정중하, 하동운 사원들의 급여와 보너스값 유재식 사원의 보너스,급여로 변경

```sql
SET (SALARY,BONUS) = (SELECT SALARY 	
                      FROM EMP_SALARY 
							 WHERE EMP_NAME = '유재식')
WHERE EMP_NAME IN('노옹철','전형동','정중하','하동운');
```

#### ASIA 지역에서 근무하는 사원들의 보너스를 0.3으로 변경

* ASIA지역에 근무하는 사원들의 사번 조회한뒤 서브쿼리로 넣어준다.

```sql
UPDATE EMP_SALARY 
SET BONUS = BONUS * 0.3 
WHERE
	( SELECT EMP_ID FROM EMP_SALARY JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID ) --> LOCATION_ID가져오기
	JOIN LOCATION ON ( LOCATION_ID = LOCAL_CODE ) WHERE LOCAL_NAME LIKE 'ASIA%'; )
```

