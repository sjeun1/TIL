
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
