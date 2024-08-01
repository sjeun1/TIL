
## 영속성 컨텍스트 (Persistence Context)
인스턴스로 존재하는 엔티티를 관리하고 영속화 시키는 영역이다. <br>
EntityManager 는 entity를 관리하고 db 의 데이터를 관리(CRUD) 할 수 있는 객체를 말한다. <br>
entityManager 를 생성하면 그 안에 persistenctContext 가 있다.  <br>
 <br> <br>

### 영속성 컨텍스트의 특징
-&nbsp; 1차 캐시<br>
영속성 컨텍스트 내부에 있는 캐시로 메모리에 있는 1차캐시에 먼저 엔티티를 찾고 없으면 db 에서 조회하고 1차 캐시에 저장된다 <br>
-&nbsp; 동일성 보장<br>
엔티티의 동일성을 보장한다. <br>
-&nbsp; 트랜잭션을 지원하는 쓰기 지연<br>
persist() 된 엔티티들을 보관하여 트랜잭션 flush() 를 실행하게 될 때 내부 쿼리 저장소에 insert 되는 sql 을 한번에 db 에 전송한다. <br>
-&nbsp; 변경 감지<br>
영속성 컨텍스트가 관리하는 영속상태인 엔티티를 조회해서 데이터를 변경하면 자동으로 update 가 된다. <br>
-&nbsp; 지연 로딩<br>
필요한 시점에 연관된 객체의 데이터를 불러온다. <br>
 <br> <br> 

### 엔티티의 생명주기
비영속 : 영속성 컨텍스트와 전혀 관계없는 새로운 상태 <br>
영속 : 영속성 컨텍스트에 관리되는 상태 <br>
준영속 : 영속성 컨텍스트에 저장되었다가 분리된 상태 <br>
삭제 : 삭제된 상태  <br>

```
public static void main(String[] args) {
    EntityManagerFactory emf = Persistence.createEntityManagerFactory("hello");

    EntityManager em = emf.createEntityManager();

    EntityTransaction tx = em.getTransaction();
    tx.begin();

    try {
        Member member = new Member();		// 비영속
        member.setId(100L);
        member.setName(“SJ);

        em.persist(member);				// 영속 (find() 도 동일 )
	em.detach(member);				// 준영속 (clear() / close() 도 동일)


	em.remove(member);				// 삭제 

        tx.commit();
    } catch (Exception e) {
        tx.rollback();
    } finally {
        em.close();
    }

    emf.close();
}
```
