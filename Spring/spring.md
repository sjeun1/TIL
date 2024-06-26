
## Spring


Servlet container 를 띄우고 servlet 을 등록

스프링 mvc 를 사용한다면 바인딩이 어떻게 이루어지는지 확인이 필요,

Servlet 기술만 사용해서 Servlet container 안에 프론트 컨트롤러 만드는 것 

스프링 컨테이너에 빈(helloController) 를 생성

Application context == spring container ?

spring container 는 어떤 타입의 오브젝트를 한번만 만들어주고 재사용하는 기능을 제공
spring container == 싱글톤 레지스트리라고도함 

소스코드 레벨에서는 의존 안하더라도 런타임 레벨에서 

Di에는 assembler 가 필요.

spring container 가 하는 역할은 메타데이터를 주면 그걸 클래스의 싱글톤 오브젝트를 만들고,  다른 의존할 오브젝트가 있다면 그 오브젝트를 주입해주는 작업까지 수행해줌.

—————————————————

dispatcherServlet 

디스패처 서블릿은 HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 프론트 컨트롤러(Front Controller)라고 정의

Class level 먼저 참조하고 method level 확인

@RestController 를 class 에 붙이면 기본적으로 @ResponseBody 라고 붙어있는 거라고 생각하면 됨 (=view 없어도 404 안뜨고 문자열 그대로 넘겨줌)


—————————————————

?!  템플릿 메소드 / 팩토리 메소드

인터페이스를 두고 di 가 적용되도록 구형 클래스를 따로 만들었는데 이걸 그대로 리턴하네? 
Public HelloService helloService() {
	return new SimpleHelloController();
}

인터페이스 타입으로 리턴하세요.**

@Configuration 이 붙은 class 는 구성 정보를 가지고 있는 class 다 라는 걸 class level 에 붙여줌.  spring container 가 보고 bean annotation 이 붙은 factory method 가 있구나 인식을 함 

@Configuaration 이 붙은 class 는 AnnotationConfig 를 이용하는 Application Context 에 처음 등록된다

@Component 를 붙여주면 Spring container 에 들어갈 component 라고 등록을 해주는 거고 bean 으로 사용이 가능하다. 

@ConponentScan 은 @Component 찾아서 bean 으로 등록해달라고 전달이 가능함. 하위패키지를 찾아서 등록. 

장점은 매번 구성정보를 등록해줄 필요 없이 component 만 붙여주면 된다. 
단점은 프로젝트가 커지면 정확하게 어떤 게 bean 으로 등록되는지 확인이 어려울 수 있다. 
패키지 구성을 잘하고, 모듈을 잘 나눠서 개발하면 어렵지 않게 파악되기 때문에 단점보단 장점이 더 크다.



—————————————————

Servlet 을 등록한다 의 의미 
웹 애플리케이션 서버에서 특정 URL 패턴과 Servlet 클래스를 연결하여 해당 URL 요청을 받을 때 그 요청을 해당 Servlet이 처리하도록 설정하는 것

?! 빈의 라이프사이클 메소드 ?! 

DispatcherServlet 의 type hierarchy

 Meta annotation 


