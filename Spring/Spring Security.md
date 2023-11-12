

### Spring Security 란
Spring boot 3.X 사이드 프로젝트를 시작하면서 예전에 했던 부분과 다른 부분이 있는 것 같아 시큐리티를 다시 공부 및 정리 해보았다.  <br> 
Spring Security는 엔터프라이즈 애플리케이션을 위한 인증, 권한 부여 및 기타 보안 기능을 제공하는 Java/Java EE 프레임워크이다<br>
모든 request 는 filter 를 거쳐서 동작한다. 
인증 -> 인가 순 <br>
인증 (Authentication) : 유저가 누구인지 확인 (회원가입, 로그인) <br>
인가 (Authorization) : 유저의 권한 확인, 허락 <br>

<img width="703" alt="image" src="https://github.com/sjeun1/TIL/assets/62210870/c0d7f34a-e155-4581-999b-405777528074"> <br>

1. client(user)가 로그인을 시도한다
2. AuthenticationFilter는 AuthenticationManager, AuthenticationProvider(s), UserDetailsService를 통해 DB에서 사용자 정보를 읽어온다.
3. UserDetailsService는 로그인한 ID에 해당하는 정보를 DB에서 읽어들여 UserDetails를 구현한 객체로 반환한다. UserDetails정보를 user의 session 에 저장을 한다.
4. 스프링 시큐리티는 인메모리 세션저장소인 SecurityContextHolder 에 UserDetails 정보를 저장한다.
5. client(user)에게 session ID(JSESSION ID) 와 함께 응답을 내려준다.
6. 이후 요청에서는 요청 쿠키에서 JSESSION ID정보를 통해 이미 로그인 정보가 저장되어 있는 지 확인한다. 이미 저장되어 있고 유효하면 인증 처리를 해주게 된다.
<br>

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

<br>


--- 
참고 / 출처 : https://spring.io/projects/spring-security#overview <br>

https://docs.spring.io/spring-security/reference/5.8/servlet/integrations/cors.html#page-title<br>

http://www.springbootdev.com <br>

https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter <br>

https://docs.spring.io/spring-security/site/docs/5.7.0-M2/api/org/springframework/security/config/annotation/web/configuration/WebSecurityConfigurerAdapter.html#configure%25org.springframework.security.config.annotation.web.builders.WebSecurity%29 <br>
