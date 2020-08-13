# 형변환 함수

## 포멧

### 년도

| 포맷 | 출력  \(2020년\) |
| :---: | :---: |
| 'YYYY' | 2020 |
| 'RRRR' | 2020 |
| 'YY' | 20 |
| 'RR' | 20 |
| 'YEAR' | TWENTY TWENTY |

### 월

| 포맷 | 출력 \(8월\) |
| :---: | :---: |
| 'MM' | 08 |
| 'MON' | 8월 |
| 'MONTH' | 8월 |
| 'RM' | VIII   \(로마자\) |

### 일

| 포맷 | 출력 \(4일\) |
| :---: | :---: |
| 'D' | 3  \(요번주 기준\) |
| 'DD' | 04 \(요번달 기준\) |
| 'DDD' | 217 \(요번년 기준\) |

### 요일

| 포멧 | 출력 |
| :---: | :---: |
| 'DY' | 화 |
| 'DAY' | 화요일 |



## 

## NUMBER \| DATE =&gt; CHARACTER

### 

### TO\_CHAR

* 숫자형 또는 날짜형 데이터를 문자형 타입으로 변환

```sql
TO_CHAR(NUMBER|DATE,[포멧])
```

#### NUMBER -&gt; CHARACTER

```sql
SELECT
	TO_CHAR( 1234 ) 
FROM
	DUAL;
	
//포멧
SELECT
	TO_CHAR( 1234,'00000' )-- '01234' 5칸의 0 공간확보후 오른쪽 정렬 (자리수는 넉넉히) 
FROM
	DUAL;
	
SELECT
	TO_CHAR( 1234,'99999' )-- ' 1234' 5칸의 공백확보후 오른쪽 정렬 
FROM
	DUAL;
	
SELECT
	TO_CHAR( 1234,'L99999' )  --￦ 1234 (L은 지역의 화폐)
FROM
	DUAL;
```

#### 

#### DATE -&gt; CHARACTER

```sql
--년 월 일
SELECT
	TO_CHAR( SYSDATE, 'YY-MM-DD' ) --  2020-08-04
SELECT
	TO_CHAR( SYSDATE, 'YYYY"년" MM"월" DD"일"' )  --2020년 08월 04일
FROM
	DUAL;
	
--시 분 초
SELECT
	TO_CHAR( SYSDATE, 'AM HH:MI:SS' ) -- 오후 06:12:20
SELECT
	TO_CHAR( SYSDATE, 'HH24:MI:SS' ) --  18:12:20
FROM
	DUAL;
```



## NUMBER \| CHARACTER =&gt; DATE

### TO\_DATE

* 숫자형 또는 문자형 데이터를 날짜타입으로 변환

```sql
TO_DATE(NUMBER|CHARACTER,[포멧])
```

```sql
SELECT
	TO_DATE( '20100101' ) --10/01/01
SELECT
	TO_DATE( '20200101', 'YYYYMMDD' )  --10/01/01
FROM
	DUAL;
```



### RR & YY 차이

```sql
	--YY
SELECT
	TO_DATE( '980630', 'YYMMDD' ) 
	--TODATE 함수 통해 DATE형식으로 변환시 YY포맷:무조건 현재세기
FROM
	DUAL;
	
	--RR
SELECT
	TO_DATE( '980630', 'RRMMDD' ) 
	--50이상이면 이전세기, 50미만 현재세기
FROM
	DUAL;
```



## CHARACTER =&gt; NUMBER

* 문자형 데이터를 숫자타입으로 변환

```sql
TO_NUMBER(CHARACTER,[포맷])
```

```sql
SELECT
	'123' + '123' --246
FROM
	DUAL;
SELECT
	TO_NUMBER( '10,000,000' ) --ERROR , ','문자값 섞여있어서
FROM
	DUAL;
SELECT
	TO_NUMBER( '10,000,000', '99,999,999' ) 
FROM
	DUAL;
```

{% hint style="danger" %}
'1000'과 같이 숫자로 이루어진 문자는 자동형변환 되지만,

'100,0000'과같이 문자가 섞이면 형변환을 포맷지정과 함께 해야합니다.
{% endhint %}

