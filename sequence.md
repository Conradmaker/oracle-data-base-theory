# SEQUENCE

## SEQUENCE

* 자동으로 번호 발생기켜주는 역할을 하는 객체
* 정수값을 자동으로 순차적으로 생성해준다.



## 표현법

```sql
CREATE SEQUENCE 시퀀스명
[START WITH 숫자]  -->처음 발생시킬 시작값 지정
[INCREMENT BY 숫자] -->몇씩 증가시킬 건지
[MAXVALUE 숫자] -->최대값 지정
[MINVALUE 숫자] -->최소값 지정
[CYCLE | NOCYCLE] -->값 순환 여부 지정
[CACHE 바이트크기 | NOCACHE] -->캐시메모리 할당(기본값 20BYTE) 
```

#### 캐시메모리

* 미리 발생될 값들을 생성해서 저장해둠매번 호출할때마다 새로 번호를 생성하는것보다 캐시메모리에 미리 생성된걸 가져가 쓰는 것이 더 빠르다.



## 사용

### 자주쓰는 시퀀스명

* 테이블 : TB\_
* 뷰 : VW\_
* 시퀀스 : SEQ\_
* 트리커 : TRG\_

```sql
CREATE SEQUENCE SEQ_EMPNO
START WITH 300
INCREMENT BY 5
MAXVALUE 310
NOCYCLE
NOCACHE;
```

### SEQUENCE 사용 구문

#### 시퀀스명.CURVAL

* 현재 시퀀스의 값 \(마지막의 NEXTVAL의 값\)

#### 시퀀스명.NEXTVAL

* 시퀀스 값을 증가시키고 증가된 시퀀스의 값
* 기존 시퀀스의 값에서 INCREMENT BY값만큼 증가된 값
* == 시퀀스명.CURVAL + INCREMENT BY 값

## 변경

표현법

```sql
CREATE SEQUENCE 시퀀스명
[INCREMENT BY 숫자] -->몇씩 증가시킬 건지
[MAXVALUE 숫자] -->최대값 지정
[MINVALUE 숫자] -->최소값 지정
[CYCLE | NOCYCLE] -->값 순환 여부 지정
[CACHE 바이트크기 | NOCACHE] -->캐시메모리 할당(기본값 20BYTE) 
```

{% hint style="warning" %}
START WITH은 사용할 수 없습니다.
{% endhint %}

## 삭제

```sql
DROP SEQUENCE SEQ_EMPNO;
```

