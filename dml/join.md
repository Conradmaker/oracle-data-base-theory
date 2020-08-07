# JOIN

## JOIN

* 하나 이상의 테이블에서 데이터를 조회하기 위해 사용하고 수행 결과는 하나의 Result Set으로 나옴
* 두개 이상의 테이블에서 데이터를 조회하고자 할때 사용되는 구문
* 조회결과는 하나의 결과물로 나옴

{% hint style="info" %}
관계형 데이터베이스는 최소한의 데이터로 각각의 테이블에 담고있다

\(중복의 최소화\)
{% endhint %}

* 무작정 JOIN을 해서 같이 조회하는 것이 아니라
* 테이블간 '연결고리'의 칼럼 데이터를 '매칭'시켜서 조회해야한다.

## 한눈에 보기

JOIN은 크게 '오라클 전용 구문'과 'ANSI 구문' 으로 나뉜다.

<table>
  <thead>
    <tr>
      <th style="text-align:center">ORACLE &#xAD6C;&#xBB38;</th>
      <th style="text-align:center">ANSI &#xAD6C;&#xBB38;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:center">ORACLE DBMS</td>
      <td style="text-align:center">ORACLE + &#xB2E4;&#xB978; DBMS</td>
    </tr>
    <tr>
      <td style="text-align:center">&#xB4F1;&#xAC00;&#xC870;&#xC778;(EQUAL JOIN)</td>
      <td style="text-align:center">&#xB0B4;&#xBD80;&#xC870;&#xC778; (INNER JOIN)</td>
    </tr>
    <tr>
      <td style="text-align:center">--</td>
      <td style="text-align:center">&#xC790;&#xC5F0;&#xC870;&#xC778; (NATUAL JOIN)</td>
    </tr>
    <tr>
      <td style="text-align:center">&#xD3EC;&#xAD04;&#xC870;&#xC778;(LEFT OUTER)</td>
      <td style="text-align:center">&#xC67C;&#xCABD; &#xC678;&#xBD80;&#xC870;&#xC778;(LEFT OUTER JOIN)</td>
    </tr>
    <tr>
      <td style="text-align:center">&#xD3EC;&#xAD04;&#xC870;&#xC778;(RIGHT OUTER)</td>
      <td style="text-align:center">&#xC624;&#xB978;&#xCABD; &#xC678;&#xBD80;&#xC870;&#xC778;(RIGHT OUTER
        JOIN)</td>
    </tr>
    <tr>
      <td style="text-align:center">X</td>
      <td style="text-align:center">&#xC804;&#xCCB4; &#xC678;&#xBD80;&#xC870;&#xC778;(FULL OUTER JOIN)</td>
    </tr>
    <tr>
      <td style="text-align:center">&#xCE74;&#xD14C;&#xC2DC;&#xC548; &#xACF1;(CARTESIAN PRODUCT)</td>
      <td style="text-align:center">&#xAD50;&#xCC28;&#xC870;&#xC778;(CROSS JOIN)</td>
    </tr>
    <tr>
      <td style="text-align:center">
        <p>&#xC790;&#xCCB4;&#xC870;&#xC778;(SELF JOIN),</p>
        <p>&#xBE44;&#xB4F1;&#xAC00; &#xC870;&#xC778;(NON EQUAL JOIN)</p>
      </td>
      <td style="text-align:center">JOIN ON</td>
    </tr>
  </tbody>
</table>

JOIN 없이 각 사원들의 사번, 사원명, 부서코드, 부서명까지 같이 조회하고 싶을때?

```sql
SELECT
	EMP_ID,
	EMP_NAME,
	DEPT_CODE 
FROM
	EMPLOYEE;
	
SELECT
	DEPT_ID,
	DEPT_TITLE 
FROM
	DEPARTMENT;
```

## 

## 등가조인  /  내부조인

* 연결시키는 칼럼의 값이 일치하는 행들만 조인되서 조회
* 즉 일치하는 값이 없는 행은 조회되지 않는다.

### EX

사번, 사원명, 부서코드, 부서명 조회

{% tabs %}
{% tab title="ORACLE" %}
```sql
--FROM절에 조회하고자 하는 테이블들을 나열 (,구분자료)
--WHERE 절에 매칭시킬 칼럼명(연결고리)에 대한 조건을 제시
--연결할 두 칼럼명이 다른 경우(EMPLOYEE:DEPT_CODE / DEPARTMENT:DEPT_ID)

SELECT
	EMP_ID,
	EMP_NAME,
	DEPT_CODE,
	DEPT_TITLE 
FROM
	EMPLOYEE,
	DEPARTMENT 
WHERE
	DEPT_CODE = DEPT_ID;
	
	--> 일치하지 않는 값은 조회에서 제외된다.
	
--연결할 두 칼럼명이 같은경우
--방법1  태이블명
SELECT
	EMP_ID,
	EMP_NAME,
	EMPLOYEE.JOB_CODE,  --중복방지
	JOB_NAME 
FROM
	EMPLOYEE,
	JOB 
WHERE
	EMPLOYEE.JOB_CODE = JOB.JOB_CODE;   --중복방지
	
--방법2 (추천) 별칭부여
SELECT
	EMP_ID,
	EMP_NAME,
	E.JOB_CODE,  
	JOB_NAME 
FROM
	EMPLOYEE E,
	JOB J
WHERE
	E.JOB_CODE = J_CODE;   
```
{% endtab %}

{% tab title="ANSI" %}
```sql
--FROM절에 기준 테이블 하나 기술한 뒤
--그 뒤에 JOIN절에서 같이 조회하고자 하는 테이블 기술(또한 매칭시킬 칼럼에 대한 조건)
-->USING구문, ON구문

--연결할 두 칼럼명이 다른경우 => JOIN ON
SELECT
	EMP_ID,
	EMP_NAME,
	DEPT_CODE,
	DEPT_TITLE 
FROM
	EMPLOYEE
JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID );

--연결할 두 칼럼명이 같은경우 => JOIN USING , JOIN ON
--ON
SELECT
	EMP_ID,
	EMP_NAME,
	E.JOB_CODE,
	JOB_NAME 
FROM
	EMPLOYEE E
	JOIN JOB J ON ( E.JOB_CODE = J.JOB_CODE )
--USING
SELECT
	EMP_ID,
	EMP_NAME,
	JOB_CODE,
	JOB_NAME 
FROM
	EMPLOYEE
	JOIN JOB USING ( JOB_CODE );
```
{% endtab %}
{% endtabs %}



칼럼출력을 안해도 비교대상이 있을경우 JOIN을 통해서 조회해야 한다.

{% tabs %}
{% tab title="ORACLE" %}
```sql
--FROM절에 조회할 테이블들 기술 , WHERE절에 매칭시키는 조건기술

SELECT
	EMP_ID,
	EMP_NAME,
	SALARY 
FROM
	EMPLOYEE E,
	JOB J 
WHERE
	E.JOB_CODE = J.JOB_CODE 
	AND JOB_NAME = '대리'
```
{% endtab %}

{% tab title="ANSI" %}
```sql
--FROM절에 테이블 하나만 기술, JOIN절에 추가로 조회하고자하는 테이블 및 조건(ON/USING)

SELECT
	EMP_ID,
	EMP_NAME,
	SALARY 
FROM
	EMPLOYEE
	JOIN JOB USING ( JOB_CODE ) 
WHERE
	JOB_NAME = '대리';
```
{% endtab %}
{% endtabs %}



### 실습

1.부서가 인사관리부인 사원들의 사번, 사원명, 보너스를 조회

2.부서가 총무부가 아닌 사원들의 사원명, 급여, 입사일

3.보너스를 받는 사원들의 사번 , 사원명, 보너스, 부서명

{% tabs %}
{% tab title="1" %}
```sql
--ORACLE
SELECT 
	EMP_ID,EMP_NAME,BONUS
FROM
	EMPLOYEE ,DEPARTMENT 
WHERE 
	DEPT_CODE = DEPT_ID 
	AND 
	DEPT_TITLE='인사관리부';
	
--ANSI
SELECT 
	EMP_ID,EMP_NAME,BONUS
FROM 
	EMPLOYEE 
JOIN 
	DEPARTMENT 
	ON (DEPT_CODE = DEPT_ID);
WHERE
	DEPT_TITLE='인사관리부';
```
{% endtab %}

{% tab title="2" %}
```sql
--ORACLE
SELECT
	EMP_NAME,
	SALARY,
	HIRE_DATE 
FROM
	EMPLOYEE,
	DEPARTMENT 
WHERE
	DEPT_CODE = DEPT_ID 
	AND DEPT_TITLE != '총무부';


--ANSI
SELECT
	EMP_NAME,
	SALARY,
	HIRE_DATE 
FROM
	EMPLOYEE
	JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID ) 
WHERE
	DEPT_TITLE != '총무부';
```
{% endtab %}

{% tab title="3" %}
```sql
--ORACLE
SELECT
	EMP_ID,
	EMP_NAME,
	BONUS,
	DEPT_TITLE 
FROM
	EMPLOYEE,
	DEPARTMENT 
WHERE
	DEPT_CODE = DEPT_ID 
	AND BONUS IS NOT NULL;
	
--ANSI
SELECT
	EMP_ID,
	EMP_NAME,
	BONUS,
	DEPT_TITLE 
FROM
	EMPLOYEE,
	DEPARTMENT
	
WHERE
	JOIN DEPARTMENT DEPARTMENT ON ( DEPT_CODE = DEPT_ID ) BONUS IS NOT NULL;
```
{% endtab %}
{% endtabs %}



## 포괄조인 / 외부조인

등가조인 / 내부조인같은 경우에는 NULL값 데이터를 조회하지 않는다. \(아래 예시\)

```sql
SELECT
	EMP_NAME,
	DEPT_TITLE,
	SALARY 
FROM
	EMPLOYEE
	JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID );
--사원이 없는 부서나 부서가 업슨ㄴ 사원은 조회되지 않는다.
```

### LEFT OUTER JOIN 

* 두 테이블 중 왼편에 기술된 테이블 기준으로 JOIN

{% tabs %}
{% tab title="ANSI" %}
```sql
SELECT
	EMP_NAME,
	DEPT_TITLE,
	SALARY 
FROM
	EMPLOYEE  --EMPLOYEE기준 (모든 사원 조회)
	LEFT JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID );
```
{% endtab %}

{% tab title="ORACLE" %}
```sql
SELECT
	EMP_NAME,
	DEPT_TITLE,
	SALARY 
FROM
	EMPLOYEE,
	DEPARTMENT 
WHERE
	DEPT_CODE = DEPT_ID ( + ) --으론쪽 테이블 (반대쪽 칼럼)에 +
```
{% endtab %}
{% endtabs %}

### RIGHT OUTER JOIN

* 두 테이블 중 오른편에 기술된 테이블 기준으로 JOIN

```sql
SELECT
	EMP_NAME,
	DEPT_TITLE,
	SALARY 
FROM
	EMPLOYEE  
	RIGHT JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID );
	         --DEPARTMENT기준 (모든 부서 조회)
```

### FULL OUTER JOIN

* 두 테이블 모두에 기술된 테이블 기준으로 JOIN
* 오라클구문 x

```sql
SELECT
	EMP_NAME,
	DEPT_TITLE,
	SALARY 
FROM
	EMPLOYEE
	FULL JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID );
```

## 카테시안 곱 / 교차조인

* 모든 테이블의 각 행들이 서로 매핑된 데이터가 조횜\(곱집합\)
* 두 테이블의 행들이 모두 곱해진 행들의 조합이 출력
* 과부하의 위험

{% tabs %}
{% tab title="ORACLE" %}
```sql
SELECT
	EMP_NAME,
	DEPT_TITLE 
FROM
	EMPLOYEE,
	DEPARTMENT;
```
{% endtab %}

{% tab title="ANSI" %}
```sql
SELECT
	EMP_NAME,
	DEPT_TITLE 
FROM
	EMPLOYEE 
	CROSS JOIN DEPARTMENT;
```
{% endtab %}
{% endtabs %}



## 자체조인 / JOIN ON

* 같은 테이블을 다시한번 조인하는 경우
* 자기 자신과 조인을 맺는 것

```sql
SELECT
	* 
FROM
	EMPLOYEE --사원에 대한 테이블
SELECT
	* 
FROM
	EMPLOYEE --사수에 대한 테이블
```

{% tabs %}
{% tab title="ORACLE" %}
```sql
SELECT
	E.EMP_ID 사원번호,
	E.EMP_NAME 사원이름,
	E.DEPT_CODE 사원부서코드,
	E.MANAGER_ID 사수번호,
	M.EMP_NAME 사수이름,
	M.DEPT_CODE 사수부서 
FROM
	EMPLOYEE E,
	EMPLOYEE M 
WHERE
	E.MANAGER_ID = M.EMP_ID;
```
{% endtab %}

{% tab title="ANSI" %}
```sql
SELECT
	E.EMP_ID 사원번호,
	E.EMP_NAME 사원이름,
	E.DEPT_CODE 사원부서코드,
	E.MANAGER_ID 사수번호,
	M.EMP_NAME 사수이름,
	M.DEPT_CODE 사수부서 
FROM
	EMPLOYEE E
	JOIN EMPLOYEE M ON ( E.MANAGER_ID = M.EMP_ID );
```
{% endtab %}
{% endtabs %}



## 등가조인

--사원별 급여의 등급

{% tabs %}
{% tab title="ANSI" %}
```sql
SELECT
	EMP_NAME,
	SALARY,
	SAL_LEVEL 
FROM
	EMPLOYEE
	JOIN SAL_GRADE ON ( SALARY BETWEEN MIN_SAL AND MAX_SAL );
```
{% endtab %}
{% endtabs %}



## 다중조인 

* 사번, 사원명, 부서명, 근무지역명 \(LOCAL\_NAME\)

```sql
SELECT * FROM EMPLOYEE; --DEPT_CODE
SELECT * FROM EMPLOYEE; --DEPT_ID   LOCATION_ID
SELECT * FROM EMPLOYEE; --          LOCAL_CODE
```

{% hint style="info" %}
주석을 작성하면 JOIN을 조금더 쉽게 알아볼 수 있을것 같다
{% endhint %}

{% tabs %}
{% tab title="ORACLE" %}
```sql
SELECT
	EMP_ID,
	EMP_NAME,
	DEPT_TITLE,
	LOCAL_NAME 
FROM
	EMPLOYEE,
	DEPARTMENT,
	LOCATION 
WHERE
	DEPT_CODE = DEPT_ID 
	AND LOCATION_ID = LOCAL_CODE;
```
{% endtab %}

{% tab title="ANSI" %}
```sql
SELECT
	EMP_ID,
	EMP_NAME,
	DEPT_TITLE,
	LOCAL_NAME 
FROM
	EMPLOYEE
	JOIN DEPARTMENT ON ( DEPT_CODE = DEPT_ID )
	JOIN LOCATION ON ( LOCATION_ID = LOCAL_CODE );
```
{% endtab %}
{% endtabs %}

위와같이 2중으로 조인을 구성할 수 있다.

{% hint style="danger" %}
순서가 바뀌게 된다면 안된다.

연결고리를 찾아 찾아 가는 것이기 때문에 순서를 잘 구성해줘야 한다.
{% endhint %}

다음 내용들을 조회해보세요

1. 사번
2. 사원명
3. 부서명\(DEPARTMENT\)
4. 직급명\(JOB\)
5. 근무지역명\(LOCATION\)
6. 근무국가명\(NATIONAL\)
7. 급여등급\(SAL\_GRADE\)

| _**TABLE**_ | _**EMPLOYEE**_ | _**DEPARTMENT**_ | _**LOCATION**_ | _**NATIONAL**_ | _**SAL\_GRADE**_ |
| :---: | :---: | :---: | :---: | :---: | :---: |
| _**EMPLOYEE**_ | DEPT\_CODE |  | JOB\_CODE |  | SALARY |
| _**DEPARTMENT**_ | DEPT\_ID | LOCATION\_ID |  |  |  |
| _**JOB**_ |  |  | JOB\_CODE |  |  |
| _**LOCATION**_ |  | LOCAL\_CODE |  | NATIONAL\_CODE |  |
| _**NATIONAL**_ |  |  |  | NATIONAL\_CODE |  |
| _**SAL\_GRADE**_ |  |  |  |  | MAN\_SAL,MAX\_SAL |

{% tabs %}
{% tab title="ORACLE" %}
```sql
SELECT--별칭부여생략해도 되지만 가독성이유
	E.EMP_ID 사번,
	E.EMP_NAME 사원명,
	D.DEPT_TITLE 부서명,
	J.JOB_NAME 직급명,
	L.LOCAL_NAME 근무지역명,
	N.NATIONAL_NAME 근무국가명,
	S.SAL_LEVEL 급여등급 
FROM
	EMPLOYEE E,
	DEPARTMENT D,
	JOB J,
	LOCATION L,
	NATIONAL N,
	SAL_GRADE S,
WHERE
	E.DEPT_CODE = D.DEPT_ID --별칭부여생략해도 되지만 가독성이유
	AND E.JOB_CODE = J.JOB_CODE 
	AND D.LOCATION_ID = L.LOCAL_CODE 
	AND L.NATIONAL_CODE = N.NATIONAL_CODE --별칭부여생략해도 되지만 가독성이유
  AND E.SALARY BETWEEN S.MIN_SAL AND S.MAX_SAL;
```
{% endtab %}
{% endtabs %}

