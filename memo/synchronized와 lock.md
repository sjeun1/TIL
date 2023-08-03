
## synchronized 메모




- executorService 
  - 비동기로 실행 하는 작업을 효율적으로 사용 할 수 있게 도와주는 자바 라이브러리
- Race condition
    - 2개 이상의 쓰레드가 공유 데이터에 엑세스할 수 있고, 동시에 접근하려고 할 때 영향을 주는 상태.
    - 하나의 쓰레드가 완료된 후에 다른 쓰레드가 데이터에 접근 할 수 있도록 하는 것.
- synchronized
    - 해당 메소드를 한 쓰레드에서만 돌리기 위해 사용.
    - Transactional annotation 을 사용하면 테스트케이스 실패
    - 하나의 서버에서만 가능.
    - 서버가 2대 이상인 경우 데이터의 접근을 여러대에서 할 수 있기 때문에 문제가 생긴다. 

1. Pessimistic Lock
    - 데이터에 Lock 을 걸어서 정합성을 맞추는 방법.
    - Exclusive lock 을 걸면 다른 트랜잭션에서는 lock 이 해제되기 전에 데이터를 가져갈 수 없음
    - 데드락이 걸릴 수 있기 때문에 주의해서 사용해야함
    - 별도의 Lock 을 거는 것이므로 성능 감소가 일어날수 있다.
2. Optimistic Lock
    - 실제로 Lock 을 이용하지 않고 버전을 이용하여 정합성을 맞추는 방법.
    - Update 수행 시 버전이 맞는 지 확인 후 업데이트를 함
    - 읽은 버전에서 수정 사항이 생긴 경우 application 에서 다시 읽은 후에 작업을 수행하여야함
    - Lock 을 안잡으므로 pessimistic Lock 보다는 성능이 좋음
    - 업데이트가 실패했을 때 예외 케이스를 별도로 입력해야함
3. Named Lock
	- 이름을 가진 metadata locking이다. 이름을 가진 lock을 획득 후 해제까지 다른 세션은 이 lock 을 획득할 수 없다.
	- transaction 이 종료될 때 lock 이 자동으로 해제 되지 않으므로 별도의 명령어로 해제를 수행하거나 선점시간이 끝나야한다.

<br>



- orElseThrow
- Save, saveandflush 차이점  : https://www.baeldung.com/spring-data-jpa-save-saveandflush
- 더티체킹
