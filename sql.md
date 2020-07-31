# SQL

cmd창에서 sqlplus를 입력해 관리자 계정을 생성한뒤 , sql Developer 을 통해 연습용 계정을 생성해보세요

```sql
[표현법] CREATE USER 계정명 IDENTIFIED BY 비밀번호;
CREATE USER Conrad IDENTIFIED BY Conrad;
```

권한부여

```sql
--새로 생성된 사용자 계정에게 최소한의 권한(CONNECT,RESOURCE) 부여
-- [표현법] GRANT 권한, 권한, .... TO 계정명;
GRANT CONNECT,RESOURCE TO KH;
```

![](.gitbook/assets/image%20%281%29.png)

주요 데이터 타입

![](.gitbook/assets/image%20%282%29.png)

### SQL\(Structured Query Language\)

관계형 데이터베이스에서 데이터를 조회하거나 조작하기 위해 사용하는 표준 검색 언어 원하는 데이터를 찾는 방법이나 절차를 기술하는 것이 아닌 조건을 기술하여 작성

분류

![](.gitbook/assets/image%20%283%29.png)



