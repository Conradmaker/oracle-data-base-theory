# CREATE

## CREATE

다양한 객체\(구조\)들을 생성하는 구문

## 테이블 생성

### 테이블

* 행\(ROW\)와 열\(COLUMN\)로 구성되는 가장 기본적이고 핵심적인 DB객체 

### 표현법

```sql
CREATE TABLE 테이블명(
	칼럼명 자료형(크기),
	칼럼명 자료형(크기),
	칼럼명 자료형(크기),
	칼럼명 자료형(크기),
	...
)
```

자료형 \(DATA TYPE\)

| 자료형 | 종류 |
| :---: | :---: |
| 문자 | CHAR\(크기\) / VARCHAR2\(크기\) |
| 숫자 | NUMBER |
| 날짜 | DATE |



{% hint style="info" %}
* CHAR\(크기\) : 최대 2000BYTE까지 저장가능 // 고정길이
* VARCHAR2\(크기\) : 최대 4000BYTE 저장가능 // 가변길이
{% endhint %}



#### 회원에 대한 데이터를 담기위한 테이블 MEMBER 생성하기

```sql
CREATE TABLE MEMBER (
	MEM_NO NUMBER,--회원번호
	MEM_ID VARCHAR2 ( 20 ),--아이디
	MEM_PWD VARCHAR2 ( 20 ),--비밀번호
	MEM_NAME VARCHAR2 ( 20 ),--회원명
	GENDER CHAR ( 3 ),--성별(남, 여)
	PHONE CHAR ( 13 ),--전화번호
	EMAIL VARCHAR2 ( 50 ),--이메일
	MEM_DATE DATE --회원가입일
)
```

### 칼럼에 주석달기

표현법

```sql
COMMENT ON COLUMN 테이블명.칼럼명 IS '주석내용';
```



### 데이터 딕셔너리

* 다양산 객체들의 정보를 저장하고 있는 시스템 테이블\(이미 있음\)

```sql
SELECT * FROM USER_TABLES;
SELECT * FROM USER_TAB_COLUMNS;
```

{% hint style="info" %}
USER\_TABLES : 이 사용자가 가지고 있는 테이블들의 전반적인 구조를 확인할 수 있는 시스템 테이블

USER\_TAB\_COLUMNS : 테이블상에 정의되어있는 모든 칼럼과 전반적인 구조를 확인할 수 있는 시스템 테이블
{% endhint %}

### 데이터 추가할 수 있는 구문

```sql
INSERT INTO 테이블명 VALUES(칼럼수만큼의데이터);
```

```sql
INSERT INTO MEMBER
VALUES
	( '1', 'yhg0337', 'l1484418', '유원근', 
		'남', '010-8731-0337', 'yhg0337@naver.com', SYSDATE );
```

이제 NULL값이 들어오지 않도록 만들어줘야한다.

## 제약조건 걸기

* 원하는 데이터값 \(유효한값\)만 유지하기 위해서 특정 칼럼에 설정하는 제약
* 데이터 무결성 보장을 목적으로 한다
* 들어올 데이터값에 문제가 없는지 자동으로 검사할 목적

### 제약조건을 부여하는 방식

* 칼럼레벨방식
* 테이블레벨방식

```sql
--칼럼 레벨 방식
칼럼명 자료형[(크기)] 제약조건
```

### NOT NULL

칼럼레벨방식만 이용가능

```sql
CREATE TABLE MEM_NOTNULL (
	MEM_NO NUMBER NOT NULL,
	MEM_ID VARCHAR2 ( 20 ) NOT NULL,
	MEM_PWD VARCHAR2 ( 20 ) NOT NULL,
	MEM_NAME VARCHAR2 ( 20 ) NOT NULL,
	GENDER CHAR ( 3 ),
	PHONE CHAR ( 13 ),
	EMAIL VARCHAR2 ( 50 ),
MEM_DATE DATE 
)
```

### UNIQUE

칼럼레벨 / 테이블레방 둘다 사용가능

* 칼럼값에 중복값을 제한하는 제약조건
* 삽입/수정시 기존에 있는 데이터값 중에 중복값이 있을 경우 오류 발생

```sql
--칼럼레벨
create TABLE MEM_UNIQUE(
MEM_NO NUMBER NOT NULL,
	MEM_ID VARCHAR2 ( 20 ) NOT NULL UNIQUE,
	MEM_PWD VARCHAR2 ( 20 ) NOT NULL,
	MEM_NAME VARCHAR2 ( 20 ) NOT NULL,
	GENDER CHAR ( 3 ),
	PHONE CHAR ( 13 ),
	EMAIL VARCHAR2 ( 50 ),
	MEM_DATE DATE 
)
--테이블레벨
create TABLE MEM_UNIQUE(
MEM_NO NUMBER NOT NULL,
	MEM_ID VARCHAR2 ( 20 ) NOT NULL,
	MEM_PWD VARCHAR2 ( 20 ) NOT NULL,
	MEM_NAME VARCHAR2 ( 20 ) NOT NULL,
	GENDER CHAR ( 3 ),
	PHONE CHAR ( 13 ),
	EMAIL VARCHAR2 ( 50 ),
	MEM_DATE DATE ,
	UNIQUE(MEN_ID)
)
```



### 제약조건명까지 이름 지어주면서 제약조건을 부여하는 표현식

#### 칼럼레벨

```sql
CREATE TABLE 테이블명(
    칼럼명 자료형(크기) [CONSTRAINT 제약조건명] 제약조건
)
```

#### 테이블 레벨 방식

```sql
CREATE TABLE 테이블명(
    칼럼명 자료형,
    칼럼명 자료형,
    [CONSTRAINT 제약조건명] 제약조건(칼럼명)
)
```

#### EX

```sql
create TABLE MEM_UNIQUE(
	MEM_NO NUMBER CONSTRAINT MEN_NO_NN NOT NULL,
	MEM_ID VARCHAR2 ( 20 ) CONSTRAINT MEN_ID_NN NOT NULL,
	MEM_PWD VARCHAR2 ( 20 ) CONSTRAINT MEN_PWD_NN NOT NULL,
	MEM_NAME VARCHAR2 ( 20 ) CONSTRAINT MEN_NAME_NN NOT NULL,
	GENDER CHAR ( 3 ),
	PHONE CHAR ( 13 ),
	EMAIL VARCHAR2 ( 50 ),
	CONSTRAINT MEM_ID_UQ UNIQUE(MEN_ID)
)
```

### CHECK

* 칼럼에 들어올 값에 대한 조건을 제시해둘 수 있음
* 해당 조건에 만족하는 데이터값만 담길 수 있다.

```sql
CREATE TABLE MEM_CHECK(
	MEM_NO NUMBER NOT NULL,
	MEM_ID VARCHAR2 ( 20 ) NOT NULL,
	MEM_PWD VARCHAR2 ( 20 ) NOT NULL,
	MEM_NAME VARCHAR2 ( 20 ) NOT NULL,
	GENDER CHAR ( 3 ) CHECK (GENDER IN ('남','여')),
	PHONE CHAR ( 13 ),
	EMAIL VARCHAR2 ( 50 ),
	UNIQUE(MEN_ID)
	-- ,CHECK (GENDER IN ('남','여'))
)
```

뿐만 아니라 NULL값도 INSERT 가능한데 , 방지하고 싶다면 NOT NULL을 넣어주면 된다.



### PRIMARY KEY

* 테이블에서 각 행의 정보를 식별하기 위해 사용할 칼럼에 부여하는 제약조건\(식별자\)
* EX\) 학번, 송장번호,주민번호
* 해당 그 칼럼에 NOT NULL + UNIQUE

{% hint style="danger" %}
절대 자신만의 고유값을 가지고 있어야 한다.
{% endhint %}

```sql
CREATE TABLE MEM_PRIMARYKEY(
	MEM_NO NUMBER PRIMARY KEY,   -->기본키
	MEM_ID VARCHAR2 ( 20 ) NOT NULL UNIQUE, -->대체키
	MEM_PWD VARCHAR2 ( 20 ) NOT NULL,
	MEM_NAME VARCHAR2 ( 20 ) NOT NULL,
	GENDER CHAR ( 3 ) CHECK (GENDER IN ('남','여')),
	PHONE CHAR ( 13 ),
	EMAIL VARCHAR2 ( 50 ),
   --CONSTRINT MEN_NO_PK PRIMARY KEY(MEN_NO)테이블선언
);
```

{% hint style="danger" %}
기본키는 테이블에 하나의 기본키만을 가지고 있어야 한다.

단 두개의 컬럼을 묶어서 기본키로 할 수는 있다.
{% endhint %}

```sql
CREATE TABLE MEM_PRIMARYKEY(
	MEM_NO NUMBER ,  
	MEM_ID VARCHAR2 ( 20 ) ,
	MEM_PWD VARCHAR2 ( 20 ) NOT NULL,
	MEM_NAME VARCHAR2 ( 20 ) NOT NULL,
	GENDER CHAR ( 3 ) CHECK (GENDER IN ('남','여')),
	PHONE CHAR ( 13 ),
	EMAIL VARCHAR2 ( 50 ),
   PRIMARY KEY(MEN_NO,MEM_ID) --복합키
);
```

### FOREIGN KEY

* 다른 테이블에 존재하는 값만 들어와야 되는 특정 칼럼에 부여하는 제약조건
* -&gt;다른 테이블을 참조한다고 표현
* -&gt;FOREIGN KEY제약조건에 의해 테이블 간의 관계가 형성됨

#### 컬럼레벨 방식

```sql
칼럼명 자료형 [CONSTRAINT 제약조건명] REFERENCES 참조할테이블명 [(칼럼명)]
```

만약 참조테이블 칼럼명을 생략하면 기본키가 선택된다.

#### 테이블레벨 방식

```sql
FOREIGN KEY(칼럼명) REFERENCES 참조할테이블명 [(칼럼명)]
```

아래와 같은 테이블이 있고 이 테이블을 참조하기 위해서는

![](.gitbook/assets/image%20%285%29.png)

```sql
CREATE TABLE MEM(
	MEM_NO NUMBER PRIMARY KEY,   
	MEM_ID VARCHAR2 ( 20 ) NOT NULL UNIQUE, 
	MEM_PWD VARCHAR2 ( 20 ) NOT NULL,
	MEM_NAME VARCHAR2 ( 20 ) NOT NULL,
	GENDER CHAR ( 3 ) CHECK (GENDER IN ('남','여')),
	PHONE CHAR ( 13 ),
	EMAIL VARCHAR2 ( 50 ),
	GRADE_ID NUMBER REFERENCES MEM_GRADE --(GRADE_CODE) 칼럼레벨방식
	--FOREIGN KEY(GRADE_ID) REFERENCES MEM_GRADE --(GRADE_CODE) 테이블레벨방식
)
```

값을 넣을때는 외래키에 있는 값만을 넣을 수 있다.\(NULL값은 허용 가능\(NOT NULL아닐경우\)\)

```sql
INSERT INTO MEM VALUES(1,'user1','pass01','홍길동',NULL,NULL,NULL,10);
```

