## 트랜잭션 (transaction)

트랜잭션이란 데이터베이스에서 여러 작업을 하나의 단위로 묶어 처리하는 기능을 의미한다. <br>
트랜잭션은 반드시 ACID 원칙을 지켜야한다. <br>
<br>
 
### ACID 원칙 <br>
- Atomicity (원자성): 모든 작업이 성공하거나 모두 실패해야 한다. <br>
- Consistency (일관성): 트랜잭션 전후에 데이터베이스는 항상 일관된 상태여야 한다. <br>
- Isolation (격리성): 동시에 실행되는 트랜잭션들은 서로의 중간 상태를 볼 수 없고, 각각의 트랜잭션은 독립적으로 수행된다. <br>
- Durability (지속성): 트랜잭션이 완료된 후에는 시스템 오류가 발생하더라도 그 결과는 영구적으로 반영되어야 한다. <br>

Java에서는 [@Transactional](https://github.com/sjeun1/TIL/blob/main/JAVA/%40transactional.md) 어노테이션을 사용해 트랜잭션을 쉽게 관리할 수 있다. <br>
<br>

### 트랜잭션 격리수준 
여러 트랜잭션이 동시에 처리될 때, 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있게 허용할지 여부를 결정하는 것이다. <br>
종류는 4가지가 있으며 모두 자동커밋이 false 인 상황에서만 적용된다. <br>

- SERIALIZABLE 
  - 가장 엄격한 격리 수준. 여러 트랜잭션이 동일한 레코드에 동시 접근할 수 없다. 그래서 제일 안전하지만 순차적으로 처리되므로 처리 성능이 떨어진다. 
- REPEATABLE READ
  - 변경 전의 레코드, 트랙잭션 번호를 undo 공간에 백업해두어 변경 전/후 데이터를 복원할 수 있으며 다른 트잰잭션 간에 접근할 수 있는 데이터를 세밀하게 제어할 수 있다. 
- READ COMMITTED
  - 커밋된 데이터만 조회 할 수 있다.
- READ UNCOMMITED
  - 커밋하지 않은 데이터도 접근할 수 있는 격리 수준이다.
 
oracle 에서는 read committed 를, mysql 에서는 repeatable read 를 default 로 사용한다. 
