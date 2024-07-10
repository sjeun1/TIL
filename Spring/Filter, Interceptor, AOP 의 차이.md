## Filter, Interceptor, AOP 의 차이
공통 업무 (세션 등 로그인 관련 처리, 권한 체크, XSS 방어, pc와 모바일 웹 분기처리, 로그, 페이지 인코딩) 들을 처리할 때 사용.
<br><br>

#### Filter 
디스패처 서블릿(Dispatcher Servlet) 에 요청이 전달 되기 전, 후의 모든 요청에 대해 작업을 처리하는 기능을 제공한다. <br>
스프링 컨텍스트 외부에 존재하여 스프링과 무관한 자원에 대해 동작한다.  <br>
요청과 응답을 거른 뒤 정제하는 역할. 앞단에서 요청 내용을 변경하거나 응답내용에 대해서 변경하는 처리가 가능하다. <br>
보통 web.xml 에 등록하고, 일반적으로 인코딩 변환, xss 방어 등의 요청에 대한 처리로 사용된다. <br>

-&nbsp;필터의 메소드<br>
Init : 서블릿 컨테이너가 생성될 때 1번 호출된다. 필터 객체를 초기화하고 서비스에 추가하기 위한 메소드이다. 이후 요청들은 doFilter 를 통해 처리된다. <br>
doFilter : url-pattern 에 맞는 http 요청이 디스패처서블릿으로 전달되기 전에 웹 컨테이너에 의해 실행되는 메소드이다. <br>
Destroy : 필터 객체를 서비스에서 제거하고 사용하는 자원을 반환하기 위한 메소드이다.<br>

-&nbsp;필터 적용<br>
1. @Component 를 적용하면 모든 url 적용<br>
2. @WebFilter(urlPatterns="/users/*")  @ServletComponentScan<br>
ServletComponentScan : Servlet 객체들을 component 위에 올려주는 어노테이션이다. 스프링 컨테이너 등록x 서블릿 컨테이너에 등록o 되어 @order("1") 가 적용이 안된다.<br>
3. @FilterRegistrationBean : 순서와 url 지정을 하고 싶으면 @FilterRegistrationBean 을 사용해야한다.<br>

(FilterChain : 필터가 여러개 모여 형성된 체인, ApplicationFilterConfig 는 필터 객체 정보를 가지고 있는 것이다.)<br>
<br><br><br>


#### Interceptor
스프링에서 제공하는 기술로써 dispatcherServlet 이 컨트롤러를 호출하기 전과 후에 요청과 응답을 참조하거나 가공할 수 있도록 스프링 컨텍스트 내부에서 controller 에 관한 요청과 응답에 대해 처리한다. <br>
여러개를 사용할 수 있고 로그인 체크, 권한 체크, 프로그램 실행시간 계산, 로그 확인 처리 등으로 사용된다. <br>

-&nbsp;인터셉터의 메소드 <br>
PreHandle : 컨트롤러 호출 전. 반환값이 true 면 다음 인터셉터를 수행, false면 인터셉터와 컨트롤러 호출안되고 로직 수행없이 끝남 <br>
PostHandle : 컨트롤러 호출 후. 컨트롤러에서 예외 발생 시 호출 안됨 <br>
AfterCompletion : view가 렌더링 된 이후 호출된다. 예외를 파라미터로 받아 어떤 예외가 발생했는지 로그 출력 가능하다. <br>
<br>

#### Aop 
Fileter, interceptor 와 다르게 메소드 전후의 지점에 자유롭게 설정이 가능하며, 주소로 대상을 구분해서 걸르는 둘과 달리 aop 는 주소, 파라미터, 에노테이션 등 여러 방법으로 대상을 지정 할 수 있다. <br>
로깅, 트랜잭션, 에러처리 등 비지니스 단 메서드에서 조금 더 세밀하게 조정하고 싶을 때 사용된다.<br>
<br><br><br><br>


### 차이점
세가지의 차이점은 실행 순서에서도 차이가 있다. <br>
Interceptor 와 filter 는 servlet 단위에서 실행되고, aop 는 메소드 앞에 proxy 패턴의 형태로 실행된다.<br>
Filter 가 가장 밖에 있고 그 다음이 interceptor, 그 안에 controller 가 실행되기 전 aop 가 존재한다. <br>
<br>
<br>



### 활용방법
-&nbsp;Filter<br>
스프링과 무관하게 전역적으로 처리해야하는 작업.<br>
공통된 보안 및 인증 (xss, csrf 방어 등)<br>
모든 요청에 대한 로깅<br>
이미지/데이터 압축 및 문자열 인코딩<br>
스프링시큐리티가 필터 기반의 인증 및 인가를 검사할 수 있음<br>
Spring mvc 에 종속적이지 않다.<br>


-&nbsp;Interceptor <br>
클라이언트 요청과 관련되어 전역적으로 처리해야하는 작업<br>
세부적인 보안 및 인증 (특정 그룹의 사용자에 대한 기능 제한, 클라이언트 요청 관련된 인증, 인가 작업)<br>
Api 호출에 대한 로깅<br>
Controller 로 전달되는 데이터 가공 시<br>
<br>
<br>
<br>
<br>
