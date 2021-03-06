# SELECT문

## SELECT문

데이터를 조회할 때 사용되는 명령어 \(DML , DQL\)

{% hint style="info" %}
RESULT SET : SELECT문을 통해 조회된 결과물
{% endhint %}

### 사용법

#### 선택한 컬럼 조회시

```sql
SELECT 조회할컬럼, 컬럼, 컬럼.. FROM 태이블명;
```

#### 태이블 전체 조회시

```sql
SELECT * FROM 태이블명;
```

예시

EMPLOYEE테이블로부터 전체 사원의 모든 컬럼정보 조회

```sql
SELECT * FROM employee;
```

EMPLOYEE테이블로부터 전체 사원들의 사번, 이름, 급여만을 조회

```sql
SELECT EMP_ID, EMP_NAME,SALARY FROM employee;
```

{% hint style="info" %}
키워드, 테이블명, 칼럼명 등등 대소문자를 가리지 않습니다.

단, 실제 담겨있는 데이터값은 대소문자 구분\)
{% endhint %}

### 

### 컬럼값을 통한 산술연산

SELECT절에 산술연산자를 기입해서 산술연산된 결과 조회가능

EMPLOYEE 테이블로부터 직원명 , 급여, 조회

```sql
SELECT EMP_NAME,SALARY FROM employee;
```

근무일수\(오늘날짜 - 입사일\)

```sql
SELECT EMP_NAME,SYSDATE - HIRE_DATE FROM employee ;
```

{% hint style="info" %}
SYSDATE는 현재의 시간을 말합니다. \(SystemDate\)
{% endhint %}



### 별칭설정

{% hint style="info" %}
AS를 붙이든 안붙이든 간에 부여하고자 하는 별칭에 특수문자 또는 띄어쓰기가 포함될 경우 반드시 " "을 붙인다.\(묶어준다\)
{% endhint %}

```sql
SELECT EMP_NAME AS 이름, SALARY AS "급여", BONUS 보너스,
       SALARY*12 "연봉(원)", //특수문자
       (SALARY+BONUS*SALARY)*12 "총 소득" //띄어쓰기
FROM EMPLOYEE;
```

실제 그 테이블에 존재하는 데이터처럼 조회가능

```sql
//EMPLOYEE 테이블로부터 사번, 사원명, 급여, 단위('원')조회

SELECT EMP_ID, EMP_NAME, SALARY, '원'
FROM EMPLOYEE;
```

DISTINCT

칼럼에 포함된 중복값을 단 한번씩만 표시하고자 할때 사용

{% hint style="danger" %}
SELECT절에 단 한개만 사용해야한다
{% endhint %}

```sql
//EMPLOYEE테이블로부터 전 사원들의 부서코드 조회 (현재 사원들이 어떤 부서에 속해있는지만)
SELECT DEPT_CODE FROM EMPLOYEE; //중복되도 모든 사원들의 부서가 출력

//EMPLOYEE테이블로부터 중복제거한 부서코드 조회
SELECT DISTINCT DEPT_CODE FROM EMPLOYEE; 

SELECT DISTINCT DEPT_CODE, JOB_CODE FROM EMPLOYEE;
//DEPT_CODE, JOB_CODE값을 묶어서 중복판별
```



## WHERE절

조회하고자 하는 테이블에서 특정 조건에 만족하는 데이터만을 조회하고자 할 때 사용하는 구문

### 표현법

```sql
SELECT 조회할칼럼, 칼럼,..
FROM 테이블명
WHERE 조건식;
```

조건식에 다양한 연산자들을 사용할 수 있다.

| 연산자 | 종류 |
| :---: | :---: |
| 대소비교 | &gt;, &lt;, &gt;=, &lt;= |
| 동등비교 | = |
| =반대 | != , ^=, &lt;&gt; |

#### 

#### EMPLOYEE 테이블로부터 급여가 400만원 이상인 사원들만 조회

```sql
SELECT * FROM EMPLOYEE WHERE SALARY >= 4000000;
```

#### 

#### EMPLOYEE테이블에서 부서코드가 D9인 사원들의 사원명, 부서코드, 급여조회

```sql
SELECT EMP_NAME, DEPT_CODE, SALARY
FROM EMPLOYEE
WHERE DEPT_CODE='D9';
```

#### EMPLOYEE테이블에서 부서코드가 D9가 아닌 사원들의 사원명, 부서코드, 급여조회

```sql
SELECT EMP_NAME, DEPT_CODE, SALARY
FROM EMPLOYEE
WHERE DEPT_CODE <> 'D9';
```

### 

### 실행순서

위 예시를 통해 알아봐요

```sql
SELECT EMP_NAME, DEPT_CODE, SALARY//3번
FROM EMPLOYEE        //1번
WHERE DEPT_CODE='D9';//2번
```



## 실습

1. EMPLOYEE테이블에서 급여가 300만원 이상인 직원들의 직원명, 급여, 입사일 조회
2. EMPLOYEE테이블에서 재직중인 직원들의 사번, 이름, 입사일 조회
3. EMPLOYEE테이블에서 직급코드가 J2인 직원들의 직원명, 급여, 보너스 조회
4. EMPLOYEE테이블에서 연봉이 5000만원 이상인 직원들의 직원명, 급여, 연봉, 입사일 조회

{% tabs %}
{% tab title="1" %}
```sql
--EMPLOYEE테이블에서 급여가 300만원 이상인 직원들의 직원명, 급여, 입사일 조회
SELECT EMP_NAME,SALARY,HIRE_DATE
FROM EMPLOYEE
WHERE SALARY >= 3000000;
```
{% endtab %}

{% tab title="2" %}
```sql
--EMPLOYEE테이블에서 재직중인 직원들의 사번, 이름, 입사일 조회
SELECT EMP_NAME,EMP_ID, HIRE_DATE
FROM EMPLOYEE
WHERE ENT_YN = 'N';
```
{% endtab %}

{% tab title="3" %}
```sql
--EMPLOYEE테이블에서 직급코드가 J2인 직원들의 직원명, 급여, 보너스 조회
SELECT EMP_NAME, SALARY,BONUS
FROM EMPLOYEE
WHERE JOB_CODE = 'J2';
```
{% endtab %}

{% tab title="4" %}
```sql
--EMPLOYEE테이블에서 연봉이 5000만원 이상인 직원들의 직원명, 급여, 연봉, 입사일 조회
SELECT EMP_NAME,SALARY,SALARY*12 연봉, HIRE_DATE
FROM EMPLOYEE
WHERE SALARY*12 >= 50000000; //WHERE절에서는 별칭 사용 불가
```
{% endtab %}
{% endtabs %}



### 논리연산자

여러개의 조건을 엮을 때 사용

| 연산자 | 의미 |
| :---: | :---: |
| AND | ~이면서, 그리고 |
| OR | ~이거나, 또는 |

#### 부서코드가 'D9'이면서 급여가 500만원 이상인 직원들의 직원명, 부서코드, 급여조회

```sql
SELECT EMP_NAME, DEPT_CODE, SALARY
FROM EMPLOYEE
WHERE DEPT_CODE = 'D9' AND SALARY >= 5000000;
```

#### 급여가 350이상이고 600만원 이하인 직원들의 직원명, 사번, 급여, 직급코드 조회

```sql
SELECT EMP_NAME, EMP_ID, SALARY, JOB_CODE
FROM EMPLOYEE
WHERE SALARY >= 3500000 AND SALARY <= 6000000
```



## BETWEEN

### 몇 이상 몇 이하인 범위에 대한 조건을 확인할때

```sql
비교대한 칼럼명 BETWEEN 조건 AND 조건
```

#### 급여가 350만원 이상, 600만원 이하인 사원들의 사원명, 사번 ,급여, 직급코드 조회

```sql
SELECT
	EMP_NAME,
	EMP_ID,
	SALARY,
	JOB_CODE 
FROM
	EMPLOYEE 
WHERE
	SALARY BETWEEN 3500000 
	AND 6000000;
```

#### 

### BETWEEN AND 연산자 DATE형식간에서도 사용 가능

#### 입사일이 '90/01/01'~'01/01/01'인 사원들의 모든 칼럼 조회

```sql
SELECT *
FROM EMPLOYEE 
WHERE HIRE_DATE BETWEEN '90/01/01' AND '01/01/01';
```



### NOT 추가

#### 그밖에 범위를 조회한다면

```sql
SELECT *
FROM EMPLOYEE 
WHERE NOT HIRE_DATE BETWEEN '90/01/01' AND '01/01/01';
```



## LIKE & WILD CARD

### 비교하려는 컬럼값이 내가 지정한 특정 패턴에 만족될 경우 조회

* 특정 패턴에 '%' '\_' 를 와일드 카드로 사용할 수 있다

### %

```sql
비교대상칼럼명 LIKE '문자%' 
--칼럼값중에 '문자' 로 '시작'되는 걸 조회

비교대상칼럼명 LIKE '%문자'
--칼럼값중에 '문자' 로 '끝'나는 걸 조회

비교대상칼럼명 LIKE '%문자%'
--칼럼값중에 '문자' 가 '포함'되는 것을 조회
```



### \_ 

```sql
비교대상칼럼명 LIKE '_문자'
--칼럼값중에 '문자'앞에 무조건 '한글자'가 올 경우 조회

비교대상칼럼명 LIKE '__문자'
--칼럼값중에 '문자'앞에 무조건 '두글자'가 올 경우 조회
```

### ex

#### 성이 전씨인 사원들 조회

```sql
SELECT EMP_NAME
FROM EMPLOYEE
WHERE EMP_NAME LIKE '전%';
```

#### 이름중 하 가 포함된 사원들의 사원명, 주민번호, 부서코드

```sql
SELECT EMP_NAME , EMP_NO , DEPT_CODE
FROM EMPLOYEE 
WHERE EMP_NAME LIKE '%하%';
```

#### 전화번호 4번째 자리가 9로 시작하는 사원들의 사번 , 사원명, 전화번호, 이메일조회

```sql
SELECT EMP_ID, EMP_NAME , PHONE, EMAIL
FROM EMPLOYEE 
WHERE PHONE LIKE '___9%';
```

이메일중 \_ 앞글자가 3글자인 이메일 주소를 가진 사원들의 사번, 이름, 이메일조회

```sql
SELECT EMP_ID , EMP_NAME , EMAIL
FROM EMPLOYEE 
WHERE EMAIL LIKE '____%';
```

{% hint style="danger" %}
 \_을 무조건 와일드 카드로 인식하기 때문에 인식안된다.
{% endhint %}

#### 해결법 \(`ESCAPE`\)

* 어떤게 와일드카드고 어떤게 데이터인지 구분지어주면 된다.
* 데이터로 인식시킬 값 앞에 임의로 나만의 와일드카드로 제시하고 나만의 와일드카드를 `ESCAPE`로 등록

```sql
SELECT EMP_ID , EMP_NAME , EMAIL
FROM EMPLOYEE 
WHERE EMAIL LIKE '___$_%' ESCAPE '$';
```



#### 김씨 성이 나닌 사원들의 사번, 사원명, 입사일 조회

```sql
SELECT EMP_ID , EMP_NAME , HIRE_DATE
FROM EMPLOYEE 
WHERE NOT EMP_NAME LIKE '김%';  --LIKE앞에도 기입 가능

```



## 실습2

1. 이름 끝이 '연'으로 끝나는 사원들의 사원명, 입사일 조회
2. 전화번호 처음 3글자가 010이 아닌 사원들의 사우너명, 전화번호 조회
3. DEPARTMENT테이블에서 해외영업부인 모든 컬럼 조회

{% tabs %}
{% tab title="1" %}
```sql
SELECT
	EMP_NAME,
	HIRE_DATE 
FROM
	EMPLOYEE 
WHERE
	EMP_NAME LIKE '%연';
```
{% endtab %}

{% tab title="2" %}
```sql
SELECT
	EMP_NAME,
	PHONE 
FROM
	EMPLOYEE 
WHERE
	PHONE NOT LIKE '010%';
```
{% endtab %}

{% tab title="3" %}
```sql
SELECT
	* 
FROM
	DEPARTMENT 
WHERE
	DEPT_TITLE LIKE '해외%';
```
{% endtab %}
{% endtabs %}



## IS NULL / IS NOT NULL

```sql
비교대상 칼럼 IS NULL : 칼럼값이 NULL일 경우

비교대상 칼럼 IS NOT NULL : 칼럼값이 NULL이 아닌 경우
```

```sql
SELECT EMP_ID , EMP_NAME, SALARY, BONUS
FROM EMPLOYEE 
WHERE BONUS = NULL;
```

{% hint style="danger" %}
NULL값비교는 단순 비교연산자로 할 수 없다.
{% endhint %}

#### 보너스를 받지 않는 사원 \(BONUS 칼럼값이 NULL인\)들의 사번, 이름, 급여 , 보너스

```sql
--보너스를 받지 않는 사원
SELECT
	EMP_ID,
	EMP_NAME,
	SALARY,
	BONUS 
FROM
	EMPLOYEE 
WHERE
	BONUS IS NULL;

--반대로 보너스 받는 사원
SELECT
	EMP_ID,
	EMP_NAME,
	SALARY,
	BONUS 
FROM
	EMPLOYEE 
WHERE
	BONUS IS NOT NULL;
```

#### 사수도 없고 부서배치도 안받은 사원의 모든 칼럼 조회

```sql
SELECT
	* 
FROM
	EMPLOYEE 
WHERE
	MANAGER_ID IS NULL 
	AND DEPT_CODE IS NULL;
```

## 

## IN

비교대상 칼럼값에 목록들 중에 일치하는 값이 있는지

```sql
비교대상칼럼 IN (값, 값, 값, ....)
```

#### 

#### 부서코드가 D6이거나 D8이거나 D5인 사원들의 사원명, 부서코드, 급여조회

#### 사용하지 않고 구현

```sql
SELECT
	EMP_NAME,
	DEPT_CODE,
	SALARY 
FROM
	EMPLOYEE 
WHERE
	DEPT_CODE = 'D6' 
	OR DEPT_CODE = 'D8' 
	OR DEPT_CODE = 'D5';
```

#### 사용하고 구현

```sql
SELECT
	EMP_NAME,
	DEPT_CODE,
	SALARY 
FROM
	EMPLOYEE 
WHERE
	DEPT_CODE IN ( 'D6', 'D8', 'D5' );
```

#### 그 외인 사원들

```sql
SELECT
	EMP_NAME,
	DEPT_CODE,
	SALARY 
FROM
	EMPLOYEE 
WHERE
	DEPT_CODE NOT IN ( 'D6', 'D8', 'D5' );
```



## 연결연산자

* 여러 컬럼값들을 하나의 컬럼인 것처럼 연결시켜주는 연산자
* 컬럼과 리터럴 \(임의의 문자열\)을 연결할 수도 있다.

```sql
SELECT 
    EMP_ID || EMP_NAME || SALARY
FROM 
    EMPLOYEE;
```

하나의 칼럼으로 출력된다.

```sql
SELECT
	EMP_ID || '번' || EMP_NAME || '의 월급은' || SALARY || '원입니다' 
FROM
	EMPLOYEE;

-- > 몇번 누구의 월급은 몇원입니다.
```



## 연산자 우선순위

1. \(\)
2. 산술연산자
3. 연결연산자
4. 비교연산자
5. IS NULL / LIKE / IN
6. BETWEEN  / AND
7. NOT
8. AND \(논리 연산자\)
9. OR \(논리 연산자\)

## ORDER BY

* SELECT문 가장 마지막에 기입하는 구문 
* 실행순서 또한 가장 마지막이다. 

### 표현법

```sql
SELECT
	조회할 칼럼, 칼럼,
	....
FROM
	조회할 테이블명 
WHERE
	조건식 
ORDER BY
	정렬시킬칼럼명|별칠|칼럼순번 ASC ( 기본값 ) / DESC
	                           NULLS (FIRST/LAST)
```

