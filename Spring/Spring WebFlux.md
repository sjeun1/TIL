## Spring WebFlux


Spring WebFlux는 Spring 5에서 새롭게 추가된 Reactive-web Application 구현을 위한 웹 프레임워크이다. <br>
기존에 가장 많이 사용하는 Spring MVC 는 서블릿 기반의 Blocking I/O 방식이다. <br>
하지만 기술 발달로 대량의 요청 트래픽을 Spring MVC 방식이 처리하지 못하는 상황이 잦아져 적은 수의 스레드로 대량의 요청을 안정적으로 처리할 수 있는 비동기 Non-Blocking I/O 의 WebFlux 가 나타났다. <br>
<br>

Spring MVC <br>
Servlet 기반의 프레임워크여서 Servlet API 를 사용하고, 아파치 톰캣과 같은 Servlet Containers 에서 Blocking I/O 방식으로 동작한다.<br>
보안은 표준 서블릿 필터를 사용하는 Spring Security 를 사용한다. <br>
데이터 엑세스는 Spring Data JDBC, Spring Data JPA, SpringData MongoDB 를 사용한다.<br>

Spring WebFlux <br>
기본 서버엔진은 Netty 이고, Jetty 나 Undertow 같은 서버엔진에서 지원하는 리액티브 스트림즈 어댑터를 통해 리액티브 스트림즈를 지원한다.
Netty 등의 서버 엔진에서 Non-Blocking I/O 방식으로 동작한다.<br>
보안은 WebFilter 를 이용해 Spring Security 를 WebFlux 에서 사용한다. <br>
데이터 엑세스는 Spring Data R2DBC 나 NoSql 모듈을 사용한다. <br>

### R2DBC
Spring Data R2DBC 는 Repository 를 사용한 데이터 엑세스 방식 뿐만 아니라 메서드를 조합하여 데이터베이스와 인터랙션 할 수 있는 R2dbcEntityTemplate 를 제공한다. R2dbcEntityTemplate 은 아래 메서드와 함께 사용 가능하다. 

![image](https://github.com/user-attachments/assets/318b9c1d-0fc6-43da-bcaf-58dabe9450a5)

```java
@Slf4j
@Validated
@Service("bookServiceV8")
@RequiredArgsConstructor
public class BookService {
      private final @NonNull R2dbcEntityTemplate template;

      public Mono<List<Book>> findBooks(@Positive long page, @Positive long size) {

        return template
                .select(Book.class)
                .count()
                .flatMap(total -> {
                    Tuple2<Long, Long> skipAndTake = getSkipAndTake(total, page, size);
                    return template
                            .select(Book.class)
                            .all()
                            .skip(skipAndTake.getT1())
                            .take(skipAndTake.getT2())
                            .collectSortedList((Book b1, Book b2) ->
                                    (int) (b2.getBookId() - b1.getBookId()));
                });
      }

      private Mono<Book> findVerifiedBook(long bookId) {
            return template.selectOne(query(where("BOOK_ID").is(bookId)), Book.class)
                      .switchIfEmpty(Mono.error(new BusinessLogicException(ExceptionCode.BOOK_NOT_FOUND)));
      }

      private Tuple2<Long, Long> getSkipAndTake(long total, long movePage, long size) {
        long totalPages = (long) Math.ceil((double) total / size);
        long page = movePage > totalPages ? totalPages : movePage;
        long skip = total - (page * size) < 0 ? 0 : total - (page * size);
        long take = total - (page * size) < 0 ? total - ((page - 1) * size) : size;

        return Tuples.of(skip, take);
      }
}
```

### 예외처리
1. onErrirResume() Operator : 에러 이벤트가 발생했을 때 에러 이벤트를 Downstream 으로 전파하지 않고, 대체 punlisher 를 통해 에러 이벤트에 대한 대체 값을 emit 하거나 발생한 에러 이벤트를 래핑한 후에 다시 에러 이벤트를 발생시키는 역할을 한다. <br>
2. ErrorWebExceptionHandler : 클래스 내에 여러 개의 Sequence 가 존재한다면 각 Sequence 마다 onErrorResume 를 추가해야되어 중복 코드가 발생 시킬 수 있어 보완하기 위해 나타난 글로벌 예외 핸들러이다.<br>

### WebClient
Spring 5 부터 지원하는 Non-Blocking HTTP request 를 위한 리액티브 웹 클라이언트로서 함수형 기반의 향상된 API를 제공한다. <br>
내부적으로 HTTP 클라이언트 라이브러리에게 HTTP request 를 위임하며, 기본 HTTP 클라이언트 라이브러리는 Reactor Netty 이다. <br>

bodyValue() : request body 를 설정한다. body(bodyInserter) 의 단축 메서드이다. <br>
retrieve() : response 를 어떤 형태로 얻을지에 대한 프로세스의 시작을 선언하는 역할을 한다. <br>
toEntity(..) : 파라미터로 주어진 클래스의 형태로 변환한 response body 가 포함된 ResponseEntity 객체를 리턴한다. <br>
bodyToMono() : response body 를 파라미터로 전달된 타입의 객체로 디코딩한다. <br>
bodyToFlux() : response body 를 파라미터로 전달된 타입의 객체로 디코딩한다. Java Collection 타입의 response body 를 수신한다. <br>

SSE, Server Sent Event 를 이용해 데이터를 streaming 할 수 있다.<br>



<br><br><br><br>
참고 (학습자료)
스프링으로 시작하는 리액티브 프로그래밍_Spring WebFlux를 이용한 Non-Blocking 애플리케이션 구현 | 황정식 저자 <br>
https://github.com/bjpublic/Spring-Reactive/blob/main/part3/chapter21/src/main/java/com/itvillage/example/WebClientExample02.java <br>
https://docs.spring.io/spring-data/r2dbc/docs/current-SNAPSHOT/reference/html/#reference <br>
