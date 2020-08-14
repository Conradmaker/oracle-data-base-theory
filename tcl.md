# TCL

TCL

TRANSACTION CONTROL LANGUAGE

트랜잭션을 제어하는 언어



* 데이터의 변경사항\(DML\)들을 하나의 트랜잭션에 묶어서 처리
* COMMIT하기 전까지의 변경사항들을 하나의 트랜잭션에 감게됨
* 트랜잭션의 대상이 되는 SQL : INSERT , UPDATE , DELETE\(DML\)

COMMIT\(트랜잭션 종료처리 후 확정\) , ROLLBACK\(트랜잭션 취소\), SAVEPOINT\(임시저장\)

* COMMIT 진행: 하나의 트랜잭션에 담겨있는 변경사항들을 실제 DB에 반영하겠다.
* ROLLBACK 진행: 하나의 트랜잭션에 담겨있는 변경사항들을 삭제한 후 마지막 COMMIT시점으로 돌아감
* SAVEPOINT 진행 : 임시저장을 하는것 즉, ROLLBACK시 SAVEPOINT까지 ROLLBACK할 수 있다.

