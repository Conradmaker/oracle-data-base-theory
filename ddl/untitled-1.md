# ALTER

## ALTER 

* 테이블을 수정하는 구문

### 표현법

```sql
ALTER TABLE 테이블명 수정할내용
```

### 수정할내용

* 칼럼 추가 / 수정 / 삭제
* 제약조건 추가 / 삭제 =&gt; 수정은 불가 \(삭제후 새로 추가해야함\)
* 테이블명 / 칼럼명 / 제약조건명 변경

## 칼럼추가 / 수정 / 삭제

### 칼럼추가\(ADD\)

```sql
ALTER 칼럼명 데이터타입 [DEFAULT 기본값]
```

```sql
--CNAME 칼럼 추가
	ALTER TABLE DEPT_COPY ADD CNAME VARCHAR2(20);
	--> 새로운 칼럼이 만들어지고 기본적으로 NULL값이 채워진다.
	
--LNAME 칼럼 추가
	ALTER TABLE DEPT_COPY ADD LNAME VARCHAR2(20) DEFAULT '한국';
	-->새로운 칼럼이 만들어지고 기본적으로 '한국'값이 채워진다.
```

### 칼럼수정

* 데이터 타입 변경:

  ```sql
  .
  ```

* 기본값 변경:

  ```sql
  .
  ```

```sql
--DEPT_COPY칼럼의 데이터 타입을 CHAR(3)으로 변경
	ALTER TABLE DEPT_COPY MODIFY DEPT_ID CHAR(3);
```

{% hint style="danger" %}
* 문자열만 담겨있다면 넘버로 바꿔서는 안된다. 
* 이미 가진 값들의 크기보다 작아질 수는 없다.
{% endhint %}

#### 다중수정

DEPT\_TITLE칼럼의 데이터 타입을 VARCHAR2\(40\)으로, 

LOCATION\_ID칼럼의 데이터 타입을 VARCHAR2\(2\)으로, 

LNAME 칼럼의 기본값을 '미국'으로

```sql
ALTER TABLE DEPT_COPY
	MODIFY DEPT_TITLE VARCHAR2(40)
	MODIFY LOCATION VARCHAR2(2)
	MODIFY LNAME DEFAULT '미국';
```

### 칼럼삭제

DROP COLUMN

```sql
DROP COLUMN 삭제할칼럼명
```

{% hint style="warning" %}
단, 최소 1개의 칼럼은 존재해야 한다.
{% endhint %}

```sql
	--DEPT_ID칼럼 지우기
	ALTER TABLE DEPT_COPY DROP COLUMN DEPT_ID;
```

#### 다중 삭제는 할 수가 없다.

```sql
--DEPT_TITLE,CNAME,LNAME 칼럼들 지우기(다중삭제)
	ALTER TABLE DEPT_COPY
	DROP COLUMN DEPT_TITLE
	DROP COLUMN CNAME
	DROP COLUMN LNAME
	---- 오류 ----
```

따로따로 작성해줘야 한다.

#### 만약 참조되고 있는 칼럼이면 삭제 불가능

```sql
ALTER TABLE DEPARTMENT DROP COLUMN DEPT_ID;
```



