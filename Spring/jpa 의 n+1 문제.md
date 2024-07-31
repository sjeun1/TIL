## Jpa의 N + 1 문제

@ManyToOne 을 사용하는 경우 N + 1 문제가 발생할 수 있다.<br>
N + 1 문제를 해결 하기 위해서 Spring Data JPA는 여러 솔루션을 제공한다. <br>
일반적으로는 fetch join 사용과 entityGraph 를 사용하여 해결할 수 있다. <br>

1. Eager fetching : fatching 하는 동안 동일한 쿼리에서 fetching 한다. 대규모 컬렉션이나 심층적인 객체 그래프가 있는 경우 즉시 페칭으로 인해 성능 문제가 발생 할 수 있다. <br>
2. Lazy fetching : 해당 속성의 getter 메서드에 액세스 할 때만 연관된 엔티티가 로드 된다. <br>
Jpa 에서 사용하는 경우 기본 lazy 페칭 동작을 유지하고 쿼리에서 join fetch 키워드를 사용하여 연관된 엔티티를 즉시 페칭 할 수 있다. <br>
기본적으로 jpa 는 @ManyToOne 연결에 대해 LAZY 페칭을 사용한다. <br>
3. Entity graph : 주석이나 xml 메타 데이터를 사용하여 엔티티 그래프를 정의할 수 있다. 쿼리에서 엔티티 그래프를 지정하면 어떤 관계를 패치해야하는지 제어할 수 있다. <br>
필요한 엔티티들만 선택적으로 명시적으로 정의하여 로딩할 수 있도록 제어하는 기능이다. <br>
<br>

&nbsp;&nbsp;&nbsp;&nbsp;3-1. Named Entity Graphs
```
@Entity
@NamedEntityGraph(
    name = "Book.author",
    attributeNodes = @NamedAttributeNode("author")
)
public class Book {
    // ...
}
```
```
@Repository
public interface BookRepository extends JpaRepository<Book, Long> {
    @EntityGraph("Book.author")
    List<Book> findAll();
}
```
<br>
&nbsp;&nbsp;&nbsp;&nbsp;3-2. Dynamic Entity Graphs
&nbsp;&nbsp;&nbsp;&nbsp;동적 엔티티 그래프를 사용하면 javax.persistence.EntityGraph API를 사용

```
@Repository
public interface BookRepository extends JpaRepository<Book, Long> {
    @Query("SELECT b FROM Book b WHERE b.genre = :genre")
    List<Book> findByGenre(@Param("genre") String genre, EntityGraph entityGraph);
}
<br>
```
<br>

4. DTO Projection : 전체 엔티티를 가져오는 대신 dto projection 을 사용한다. 필요한 정보만 포함된 DTO 를 만들면 불필요한 연결을 로드하는 것을 피할 수 있다. (데이터 하위 집합을 가져와야하거나 관계가 복잡할 때 유용하다) 
```
public interface BookProjection {
    String getTitle();
    AuthorProjection getAuthor();
}

public interface AuthorProjection {
    String getName();
}

@Repository
public interface BookRepository extends JpaRepository<Book, Long> {
    List<BookProjection> findAllProjectedBy();
}
```
