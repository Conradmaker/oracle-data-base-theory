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

