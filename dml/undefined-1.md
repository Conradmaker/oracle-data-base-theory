# 문자 관련 함수

## 문자 처리 함수

#### \(단일행 함수\)

### 

## 종류

![](../.gitbook/assets/image%20%284%29.png)



## 

## LENGTH / LENGTHB

| 함수\(해당칼럼\) | 기능 |
| :--- | :--- |
| LENGTH\(STRING\) | 해당 문자의 글자수 반환 |
| LENGTHB\(STRING\) | 해당 문자의 바이트수 반환 |

{% hint style="warning" %}
'가', '강' - 한글 한글자당 3BYTE로 취급

'A', 'a', ' 1 ', ' ! ' - 한글자당 1BYTE로 취급
{% endhint %}

#### ex

```sql
SELECT
	LENGTH( '오라클' ),    --3
	LENGTHB ( '오라클' )   --9
FROM
	DUAL;   --ORACLE의 가상태이블
```

결과값은 number 타입으로 반환된다.



## 

## INSTR

* 문자열로부터 특정 문자의 위치값 반환

```sql
INSTR(STRING, '문자', [찾을위치의시작값,[순번]])
```

찾을 위치의 시작값 \(생략가능\)

* 1  :  앞에서부터 찾겠다 \(기본값\)
* -1 :  뒤에서부터 찾겠다.

순번 \(생략가능\)



#### EX

```sql
SELECT INSTR( 'AABAACAABBAA', 'B' )   //3
	--찾을 위치의 시작값, 순번 생략시 앞에서부터 첮번째의 B의 위치값
SELECT INSTR( 'AABAACAABBAA', 'B', -1, 2 )   //9
  --뒤에서부터 2번째의 B 위치값

FROM
	DUAL;
```

```sql
SELECT
	EMAIL,
	INSTR( EMAIL, '@' )  //@의 위치값
FROM
	EMPLOYEE;
```

결과값은 number 타입으로 반환된다.



## 

## SUBSTR

* 문자열로부터 원하는 물자열을 추출
* 자바로 치면 문자열.substring\(\) 메소드와 유사

```sql
SUBSTR(STRING, POSITION, [LENGTH])
```

* STRING  :  문자타입 칼럼 또는 '문자값'
* POSITION  :  문자열을 잘라낼 시작위치값
* LENGTH  :  추출할 문자 개수 \(생략시 끝까지\)

#### EX

```sql
SELECT
	SUBSTR( 'SHOWMETHEMONEY', 7 ) //THEMONEY  7부터
SELECT
	SUBSTR( 'SHOWMETHEMONEY', 5,2 ) //ME   5에서2개
SELECT
	SUBSTR( 'SHOWMETHEMONEY', -8,2 ) //TH  음수는 뒤에서부터 값을 찾는다.
FROM
	DUAL;
```

#### 남자사원들만 조회 \( 주민번호 이용해서\)

```sql
SELECT
	EMP_NAME,
	SALARY 
FROM
	EMPLOYEE 
WHERE
	SUBSTR( EMP_NO, 8, 1 ) = '1'     --SELECT뿐만 아니라 WHERE에도 사용가능
	OR SUBSTR( EMP_NO, 8, 1 ) = '3';
```

                                  

## 

## LPAD / RPAD                                                                                                                                                                                             

* 문자에 대해 통일감 있게 보여주고자 할 때 사용

```sql
LPAD/RPAD(STRING, 최종적으로 반환할 문자의길이(BYTE),[덧붙이고자 하는문자]);
```

제시한 문자열에 임의의 문자를 왼쪽OR오른쪽에 덧붙여 최종 N길이만큼 문자열을 반환

#### EX

```sql
SELECT
	EMAIL,
	LPAD( EMAIL, 40 ) --   (공백)   이메일  두개의 합이 20BYTE
FROM
	EMPLOYEE;
```

#### 

#### 891201-2\*\*\*\*\*\* 주민번호조회 

총 글자수: 14글자

```sql
SELECT
	EMP_NAME,
	RPAD( SUBSTR( EMP_NO, 1, 8 ), 14, '*' )  --중복함수 사용도 가능하다.
FROM
	EMPLOYEE;
```



## LTRIM / RTRIM

```sql
LTRIM/RTRIM(STRING,[제거하고자하는 문자])
```

* 문자열의 왼쪽 혹은 오른쪽에서 제거하고자 하는 문자들을 찾아서 제거한 나머지 문자열을 반환
* 제거할 문자 작성안하면 공백을 제거해준다.

#### EX

```sql
SELECT
	LTRIM( '      A  B' )        --A  B 왼쪽공백들을 제거해준다.
SELECT
	LTRIM( '00012345600','0')    --12345600 왼쪽 0을 찾아 제거
FROM
	DUAL
```

```sql
SELECT
	LTRIM( 'ACABACCWG','ABC' ) --WG 
FROM
	DUAL
```

{% hint style="warning" %}
특정의 문자열을 제거 즉, 'ABC'를 지워주는 것이 아닌 'A' , 'B', 'C'를 찾아 지워주는것.
{% endhint %}

