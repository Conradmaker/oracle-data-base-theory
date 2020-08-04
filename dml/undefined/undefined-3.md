# 기타함수

## NULL처리 함수

### NVL

* 칼럼명, 해당 칼럼값이  NULL일 경우 반환할 결과값

```sql
SELECT
	EMP_NAME,
	BONUS,
	NVL( BONUS, 0 ) --BONUS칼럼값이 NULL일경우 0
FROM
	EMPLOYEE;
```

NULL값이 있는데 산술해야 하는 경우

```sql
SELECT
	EMP_NAME,
	( SALARY + NVL( BONUS, 0 ) * SALARY ) * 12 
FROM
	EMPLOYEE
```

### 

### NVL2

* 해당 컬럼값이 존재하면 결과값1으로
* 해당 칼럼값이 NULL이면 결과값2로

```sql
NVL2(칼럼명, 결과값1, 결과값2)
```

```sql
FROM
	EMPLOYEE SELECT
	EMP_NAME,
	BONUS,
	NVL2( BONUS, 0.7, 0 ) --값있으면 0.7 없으면 0
FROM
	EMPLOYEE
```



### NULLIF

* 두개의 값이 동일하면 NULL반환
* 두개의 값이 동일하지 않으면 비교대상1 반환

```sql
NULLIF(비교대상1,비교대상2)
```

## 

## 선택함수

### 

### DECODE

```sql
DECODE비교대상(칼럼명|산술연산|함수),    조건값1, 결과값1, 조건값2, 결과값2  
```

{% hint style="info" %}
swith와 비슷

```sql
switch(비교대상){
    case 조건값1 :결과값1;
    case 조건값2 :결과값2;
}
```
{% endhint %}

#### 사번, 사원명, 주민본호로부터 성별추출

```sql
SELECT
	EMP_ID,
	EMP_NAME,
	DECODE( SUBSTR( EMP_NO, 8, 1 ), '1', '남', '2', '여' )성별 --1이면 남자 2면여자
	
FROM
	EMPLOYEE;
```



### CASE WHEN THEM 구문

* DECODE선택함수와 비교하면 DECODE는 해당 조건검사시 동등비교만을 수행한다면
* CASE WHEN THEN구문으로는 특정 조건 제시시 범위지정 가능
* IF-ELSE IF 문과 비슷

```sql
CASE WHEN 조건식1 THEM 결과값1
     WHEN 조건식2 THEM 결과값2
     ...
     ELSE 결과값
END
```

