
## 스터디
이름 : JPA 스터디<br>
기간 : 2023.03.05 ~ <br>
<br>
<br>


- 테스트 코드 rollback 이 된다고 하는데 console 에 안뜸? 버전마다 다른가?<br>
- 같은 트랜잭션 안에서 실행하면 영속성 컨텍스트가 똑같음.같은 엔티티로 식별함<br>
<br>

쿼리 파라미터 찍는법<br>
org.hibernate.type: trace<br>
org.hibernate.orm.jdbc.bind: trace  // springboot 3.0.x 일때<br>
<br>

설계할 때 양방향 보단 단방향이 좋다. <br>
회원이 주문을 하니까 설계하는게 아니라 주문이 생성 되려면 회원이 있어야한다.<br>
회원을 통해서 주문이 일어난다 가 아니라 주문을 생성할때 회원이 필요하다 라는 관점이 더 맞다.<br>
회원이 주문을 참조하지 않고, 주문이 회원을 참조<br>
<br>

객체에서는 무방한데, 관계형 데이터베이스일 때는 다대다를 풀어줘야한다.<br>
카테고리 - 아이템(상품) 사이에 'category_item' 이런식으로 매핑테이블 별개로 추가해서 일대다 다대일 관계로 풀어야한다.<br>
관계형 데이터베이스에서는 FK 가 있는게 항상 n쪽에 있어야한다.  외래키가 있는 곳을 연관관계 주인으로 함. <br>
반대로 해도 상관은 없지만 1에서 관리하지 않는 n에 외래키 값이 업데이트 되므로 관리와 유지보수가 어렵고,추가적으로 별도의 업데이트 쿼리가 발생하는 성능 문제도 있다 <br>
단, 일대일은 무관하다.<br>
<br>

@ManyToMany = 실무에서는 사용 X. 운영어려움<br>
@OneToMany(mappedBy = "member")  누구에 의해서 매핑이 됐는지 <br>
@Enumerated(EnumType.ORDINAL)  Ordinal 로 사용하면 중간에 다른 enum 이 들어가면 잘안됨<br>
string 타입으로 넣어야함<br>
<br>

```java
@JoinTable(name = "category_item",
        joinColumns = @JoinColumn(name = "category_id"),
            inverseJoinColumns = @JoinColumn(name = "item_id")
)
```

<br>
inverseJoinColumns 이건 category_item 테이블에 item 쪽으로 들어가는 컬럼?을 매핑해주면 된다.<br>
<br>

#### 주의
엔티티에는 가급적 Setter 사용 X.<br>
모든 연관관계는 지연로딩으로 설정 -> 실무에서 모든 연관관rP는 지연로딩(LAZY) 로 설정.<br>
@OneToMany 랑 @ManyToOne 이랑 fetchType 이 다름.<br>
@OneToOne, @ManyToOne(fetch = FetchType.LAZY) 로 다 바꿔주어야함<br>
컬렉션은 필드에서 초기화하기 -> 하이버네이트 문제 방지 및 코드 간결을 위해<br>
<br>
<br>
<br>


...




Spring 이 BindingResult.hasErrors()

변경감지와 병합(merge)
변경감지 == dirty check
병합방식은 준영속 상태-> 영속성 컨텍스트 상태로 변경

준영속 엔티티
영속성 컨텍스트가 관리하지 않는 엔티티

병합 시 모든 속성이 변경됨
머지 안쓰는 게 좋고 머지 대신 더티체크가 써야함
엔티티를 변경할 때 항상 감지를 사용.

