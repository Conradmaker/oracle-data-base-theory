# 숫자 관련 함수

## ABS

* 절대값 구해주는 함수

```text
ABS(NUMBER)
```



## MOD

* 두 수를 나눈 나머지 값을 반환해주는 함수

```text
MOD(NUMBER,NEMBER)
```

```sql
SELECT
	MOD( 10, 3 ) --1
FROM
	DUAL;
```

ROUND

* 반올림 처리해주는 함수

```sql
ROUND(NUMBER, [위치])
```

```sql
SELECT
	ROUND( 123.456 ) --123 / 위치 생략시 기본값0 
SELECT
	ROUND( 123.456 ,-2 ) --100 / 음수 사용가능 
FROM
	DUAL;
```



## CELL

* 무조건 올림처리해주는 함수

```sql
CEIL(NUMBER)
```

```sql
SELECT
	CEIL( 123.456  ) --124 / 위치지정 불가 
FROM
	DUAL;
```



## FLOOR

* 소수점 아래 무조건 버려버리는 함수

```sql
FLOOR(NUMBER)
```

```sql
SELECT
	FLOOR( 123.956 ) --123 / 위치지정 불가
FROM
	DUAL
```



## TRUNC

* 위치지정 가능한 버림처리

```sql
TRUNC(NUMBER, [위치])
```

```sql
SELECT
	TRUNC( 123.956 ,1) --123.9 
FROM
	DUAL
```



## 

```sql

```

```sql

```



## 

* 
```sql

```

```sql

```



## 

* 
```sql

```

```sql

```

