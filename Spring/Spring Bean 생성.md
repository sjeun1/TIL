## Spring Bean 
<br>

빈(Bean)은 스프링 컨테이너에 의해 관리되는 재사용 가능한 소프트웨어 컴포넌트이다. <br>
즉, 스프링 컨테이너가 관리하는 자바 객체를 의미하며 하나 이상의 빈(Bean)을 관리하고, 필요할 때 제공하며 생명 주기를 관리한다.<br>

특징 : 객체 관리, 의존성 주입, 생명주기 관리, 스코프 관리 <br>
 

스프링 간 객체가 의존관계를 관리하도록 하는 것에 가장 큰 목적이 있다.  <br>
객체가 의존관계를 등록할 때 스프링 컨테이너에서 해당하는 빈을 찾고, 그 빈과 의존성을 만든다. <br>
 <br>


#### 1. @Configuration 안에서 @Bean 으로 등록 <br>
@Bean 을 사용해 수동으로 등록해주는 방식으로 메소드 이름으로 Bean 이름이 결정된다<br>
1개 이상의 @Bean 사용 시 @Configuration 을 명시해주어야 싱글톤이 보장된다 <br>
-&nbsp;개발자가 제어가 불가능한 라이브러리 활용 (ex. Gson 같은 외부라이브러리)<br>
-&nbsp;애플리케이션 전범위적으로 사용되는 클래스 등록 시 <br>
-&nbsp;다형성을 활용하여 여러 구현체를 등록해주어야할 때 <br>
```
package com.example;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    @Bean
    public MyService myService() {
        return new MyService();
    }
}
```

#### 2. @Component 
1은 일일히 등록해줘야하므로 2번 방식 사용<br>
component 는 구성요소라는 뜻으로 독립적인 단위 모듈을 말한다. <br>
@component 는 개발자가 직접 작성한 class 를 빈으로 만드는 것이다.<br>
해당 어노테이션이 있는 클래스들을 찾아서 자동으로 빈 등록하는 방식<br>
스프링이 권장하는 ComponentScan 을 이용한 자동 빈 방식<br>
-&nbsp;직접 개발한 클래스 일 때 권장 <br>
-&nbsp;@Component 를 이용시 Main이나 App 클래스에서 @ComponentScan 으로 컴포넌트를 찾는 탐색 범위를 지정해주어야 함. (ex.@ComponentScan(basePackages = "com.example")) <br>
-&nbsp;Config 파일에서 @ComponentScan 사용 시 basePackages 보다 basePackageClasses를 사용해서 애플리케이션이 위치한 곳부터 component scan 을 해서 좀 더 type safety 하다. 
SpringBoot 사용 시에는 @SpringBootConfiguration 하위에 포함되어있어 해당 사항 없음<br>
```
package com.example;

import org.springframework.stereotype.Component;

@Component
public class MyService {
    public void AService() {
        System.out.println("");
    }
}
```
<br>

#### 3. @SpringBootApplication
프로젝트 초기에 생성되는 파일에 이미 적용되어 있음. <br>

빈 . IoC container 가 관리하는 재사용 가능한 객체<br>
의존성 주입을 받으려면 빈으로 들어가있어야 받을 수가 있다. <br>
빈의 scope : 존재할 수 있는 범위 <br>

-&nbsp;빈으로 등록되면 장점<br>
싱글톤으로 객체를 만들어 관리하고 싶을 때도 IoC 컨테이너를 사용하면 편리<br>
의존성 관리 하기 좋음<br>
<br>





