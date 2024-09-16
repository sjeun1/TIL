

### Spring Security 란
spring security 란 인증, 권한 관리 등의 보안을 위해 사용하는 spring framework 이다. <br>
Spring boot 3.X 사이드 프로젝트를 시작하면서 예전에 했던 부분과 다른 부분이 있는 것 같아 시큐리티를 다시 공부 및 정리 해보았다.  <br> 
Spring Security는 엔터프라이즈 애플리케이션을 위한 인증, 권한 부여 및 기타 보안 기능을 제공하는 Java/Java EE 프레임워크이다<br>
모든 request 는 filter 를 거쳐서 동작한다. 
인증 -> 인가 순 <br>
인증 (Authentication) : 유저가 누구인지 확인 (회원가입, 로그인) <br>
인가 (Authorization) : 유저의 권한 확인, 허락 <br>




### spring security 구조 

<img width="703" alt="image" src="https://github.com/sjeun1/TIL/assets/62210870/c0d7f34a-e155-4581-999b-405777528074"> <br>


로그인 정보와 함께 인증 요청. <br>
AuthenticationFilter가 요청을 가로채서 UsernamePasswordAuthenticationToken의 인증용 객체를 생성. <br>
생성된 인증 토큰을 ProviderManager 에게 전달하면 ProviderManager 가 AuthenticationProvider를 조회하여 인증을 시도. <br>
UserDeatailService 의 loadUserByUserName 메서드는 db 에서 사용자를 조회 한 후, Spring security 에 필요한 유저 이름, 비밀번호, 권한이 포함된 UserDetails 객체로 반환. <br>

AuthenticationProvider는 넘겨 받은 UserDetails 과 입력된 유저 정보(비밀번호) 와 비교하여 인증.  <br>
다시 최초의 AuthenticationFilter에 Authentication 객체를 생성하고 SecurityContextHolder 에 저장. <br>
SecurityContextHolder 는 시큐리티의 인메모리 세션 저장소 역할을 하며, 인증된 사용자 정보를 담고 있음. <br>
이 후 서버가 세션ID (JSESSIONID) 를 생성하고, 클라이언트에게 응답으로 반환. <br>
클라이언트는 이후 요청을 할때마다 쿠키에 세션ID(JSESSIONID) 를 담아 전송. <br>
이미 로그인 된 사용자라면 SecurityContextHolder 에서 해당 세션에 저장된 인증정보를 사용하여 인증 처리를 함 <br>

<br><br>

### 설정 시 변경 사항
-antMatchers(), mvcMatchers(), regexMatchers() -> requestMatchers() (또는 securityMatchers()) 로 변경 <br>
-authorizeRequests() -> authorizeHttpRequests() 로 변경 <br>
-WebSecurityConfigurerAdapter 를 extend 하는 방식이 많았는데 Deprecated 가 되어있었다. (SecurityFilterChain 를 권장) <br>
-WebSecurity, HttpSecurity에 모두 설정을 한 경우 WebSecurity가 HttpSecurity보다 우선적으로 고려되기 때문에 HttpSecurity에 인가 설정은 무시된다. (6에서는 HttpSecurity 에서 등록하는 걸 권장) <br>
-UserDetailsService 등록 방식도 스프링 컨테이너에만 등록 <br>
 <br>

### CORS

HTTP 요청 할때 다른 도메인의 요청은 불가하다. <br>
요즘에는 SPA 방식으로 Front 와 Back 간의 도메인이 다른 경우가 있어서 CORS 정책이 필요하다. <br>

#### CORS 종류
1. Simple
    - GET, POST, HEAD 중의 한가지 Method 사용
2. Preflight
3. Credential
4. Non-Credential

#### CORS 부분 
<img width="937" alt="image" src="https://github.com/sjeun1/TIL/assets/62210870/fed632ee-292d-406a-8469-20fbb1700992">
#### CSRF 설정 관련
설정이 필요한 경우 <br>
웹 페이지에서 폼을 통한 데이터 전송이 있는 경우. <br>
세션 기반 인증을 사용하는 경우. <br>
쿠키 기반 인증을 사용하는 경우. <br>
ex) http.csrf().and() <br>

설정이 필요하지 않은 경우 <br>
API 서버에서 주로 JWT 또는 Bearer Token을 사용하는 경우. <br>
RESTful API만을 제공하며, 상태 비저장 방식의 요청이 대부분일 때. <br>
csrf().ignoringAntMatchers("/api/**") <br>
 <br>
<br>


### HttpSecurity, WebSecurity 의 차이
HttpSecurity 는 웹 어플리케이션의 http 요청으로 보안 규칙을 설정한다. <br>
특정 url 패턴에 대한 인증/인가, cors, csrf, oath2, 세션 관리 등의 보안 설정의 세부적인 규칙을 정의하는데에 사용된다. <br>
ex) /room 경로는 인증 필요하지만 /login 경로는 인증 없이 접근 가능 <br>
<br>

WebSecurity는 HttpSecurity의 상위 개념으로 전역적인 보안 설정을 다룬다. <br>
시큐리티가 적용되지 않을 리소스나 요청 경로를 설정한다. <br>
보안과 전혀 관련 없는 리소스들을 전역적으로 제외할 때 사용하고, 그 외에는 httpSecurity 를 사용하는 것이 좋다. <br>
ex) /static/** 경로는 보안 필터를 적용하지 않게 한다. <br>
<br>


--- 
참고 / 출처 : https://spring.io/projects/spring-security#overview <br>

https://docs.spring.io/spring-security/reference/5.8/servlet/integrations/cors.html#page-title<br>

http://www.springbootdev.com <br>

https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter <br>

https://docs.spring.io/spring-security/site/docs/5.7.0-M2/api/org/springframework/security/config/annotation/web/configuration/WebSecurityConfigurerAdapter.html#configure%25org.springframework.security.config.annotation.web.builders.WebSecurity%29 <br>
