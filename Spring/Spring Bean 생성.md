# Spring Bean 생성

1. @Configuration 안에서 @Bean 으로 등록 <br>
@Bean 을 사용해 수동으로 등록해주는 방식으로 메소드 이름으로 Bean 이름이 결정<br>
1개 이상의 @Bean 사용 시 @Configuration 을 명시해주어야 싱글톤이 보장됨<br>
-&nbsp;개발자가 제어가 불가능한 라이브러리 활용 (ex. Gson 같은 외부라이브러리)<br>
-&nbsp;애플리케이션 전범위적으로 사용되는 클래스 등록 시 <br>
-&nbsp;다형성을 활용하여 여러 구현체를 등록해주어야할 때 <br>
<br>

2. @Component 
해당 어노테이션이 있는 클래스들을 찾아서 자동으로 빈 등록하는 방식<br>
스프링이 권장하는 ComponentScan 을 이용한 자동 빈 방식<br>
-&nbsp;직접 개발한 클래스 일 때 권장 <br>
-&nbsp;@Component 를 이용시 Main이나 App 클래스에서 @ComponentScan 으로 컴포넌트를 찾는 탐색 범위를 지정해주어야 함. <br>
SpringBoot 사용 시에는 @SpringBootConfiguration 하위에 포함되어있어 해당 사항 없음<br>
<br>



