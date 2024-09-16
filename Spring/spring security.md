## spring security
spring security 란 인증, 권한 관리 등의 보안을 위해 사용하는 spring framework 이다. <br>
스프링 시큐리티를 오랜만에 다시 사용해보면서 간단하게 정리해보았다. (jpa + spring security + jwt 환경에서 사용하였다. ) <br>
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
