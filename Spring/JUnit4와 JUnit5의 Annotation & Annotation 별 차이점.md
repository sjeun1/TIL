# JUnit4와 JUnit5의 Annotation & Annotation 별 차이점 
<br>

#### @WebMvcTest 와 @SpringBootTest 차이점
WebMvcTest : controller 테스트 하는 어노테이션. http 요청, 응답은 목을 이용해 가짜로 연결함.<br>
스프링 웹 어플리케이션 컨텍스트 초기화, MockMvc, MVC 레이어 관련 컨텍스트를 불러옴<br>
서버에서 컨트롤러 테스트 할 때, 통합테스트 시 사용 권장.<br>
SpringBootTest : 웹 어플리케이션 컨텍스트와 설정을 모두 불러와 실제 웹 서버에 연결을 시도함. <br>
이런 경우 MockMvc가 아닌 RestTemplate 사용. 또는 TestRestTemplate.<br>
간단한 단위테스트 시 권장 <br>
<br>

#### @Mock, @MockBean
Mock &nbsp; : &nbsp; 모키토 라이브러리, Constructor Injection 을 이용한 Bean 테스트 시  <br>
MockBean 스프링 부트 테스트 패키지 내에 위치 <br>
Mock 객체는 실제 모듈과 비슷하게 동작하도록 하는 가짜 객체. 진짜 객체로 동작하기 어려운 경우 사용 <br>
MockBean 의 경우 Mock 객체들을 ApplicatiionContext 넣어주고 동일한 타입의 Bean 이 존재할 때 MockBean 으로 교체 <br>
Spring Boot Container 가 필요할 때 Bean 이 Container 에 존재해야 한다면 @MockBean 을 사용. <br>
<br>

#### JUnit4 &nbsp;&nbsp; /&nbsp;&nbsp; JUnit5
@RunWith(SpringRunner.class) &nbsp;&nbsp; /&nbsp;&nbsp; @ExtendWith(SpringExtension.class)<br>
: 테스트 실행 방법을 확장할 때 사용<br>
<br>

#### @BeforeClass (JUnit4) &nbsp;&nbsp; /&nbsp;&nbsp; @BeforeAll (JUnit5)
: 테스트 클래스의 모든 테스트 메서드 실행 되기 전에 실행<br>
<br>

#### @AfterClass (JUnit4) &nbsp;&nbsp; /&nbsp;&nbsp; @AfterAll (JUnit5)
: 테스트 클래스의 모든 테스트 메서드 실행 된 후에 실행<br>
<br>

#### @Before (JUnit4) &nbsp;&nbsp; /&nbsp;&nbsp; @BeforeEach (JUnit5)
: 테스트 클래스의 각 테스트 메서드를 실행 하기 전에 실행<br>
<br>

#### @After (JUnit4) &nbsp;&nbsp; /&nbsp;&nbsp; @AfterEach (JUnit5)
: 테스트 클래스의 테스트 메서드가 실행 된 후에 실행<br>
<br>

#### @Ignore (JUnit4) &nbsp;&nbsp; /&nbsp;&nbsp; @Disabled (JUnit5)
: 테스트 클래스 또는 메서드를 비활성화할 때 사용<br>
<br>

#### @Test (공통)
: 클래스의 테스트 케이스. 4에서는 public 선언, 5에서는 public 간주하므로 선언 X<br>
<br>



