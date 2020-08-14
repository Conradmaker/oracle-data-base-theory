# DCL

## DCL

DATA CONTROL LANGUAGE

데이터를 제어하는 언어

* 계정에게 시스템권한 / 객체접근권한을 부여\(GRANT\)하거나 회수\(REVOKE\)하는 언어

## 권한

* 시스템권한 : DB에 접근하는 권한, 객체를 생성할 수 있는 권한
* 객체접근권한 : 특정 객체를 조작할 수 있는 권한

### 시스템권한 종류

* CREATE SESSSION : 계정에 접속할 수 있는 권한
* CREATE TABLE : 테이블을 생성할 수 있는 권한
* CREATE VIEW : 뷰를 생성할 수 있는 권한
* CREATE SEQUENCE : 시퀀스를 생성할 수 있는 권한
* CREATE USER : 계정 생성할 수 있는 권한

표현법

```text
GRANT 권한1, 권한2,.... TO 계정명
```

계정에 테이블을 생성할 수 있는 CREATE TABLE권한부여

```text
GRANT CREATE TABLE TO 계정명;
```

추가적으로

```text
ALTER USER 계정명 QUOTA 2M ON SYSTEM;
```

위 코드까지 입력해줘야 태이블을 생성할 수 있습니다.

계정에게 다른계정.EMPLOYEE테이블에 조회권한

```text
GRANT SELECT ON 계정명.테이블명 TO 부여할계정
```

