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
