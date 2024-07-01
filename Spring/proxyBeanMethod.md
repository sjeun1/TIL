## proxyBeanMethod

proxyBeanMethod 란, 스프링 프레임워크에서 메서드 레벨의 빈 정의를 프록시할지 여부를 결정하는 속성이다. <br>
주로 @Configuration 클래스에서 사용된다.<br>
@Bean 메서드를 호출할 때 프록시를 통해 호출할지 직접 호출할지를 제어한다. <br>
Default 는 true 로, @Bean 메서드는 프록시를 통해 호출 된다. <br>

#### proxyBeanMethods = true
```
@Configuration
public class AppConfig {

    @Bean
    public ServiceA serviceA() {
        return new ServiceA(serviceB());
    }

    @Bean
    public ServiceB serviceB() {
        return new ServiceB();
    }
}
```

이경우 싱글톤 빈을 보장하고 [순환 종속성](https://github.com/sjeun1/TIL/blob/main/Spring/%EC%88%9C%ED%99%98%EC%A2%85%EC%86%8D%EC%84%B1.md)을 해결하기 위해 사용된다.  <br>
ServiceA 빈을 생성할 때 serviceB() 메서드는 프록시를 통해 호출되므로 이미 생성된 serviceB 빈이 반환된다. <br>
<br><br>
<br>

#### proxyBeanMethods = false
false 로 비활성화 할 경우에는 프록시를 사용하지 않게 된다.<br>
이 경우, 각 @Bean 메서드는 독립적으로 호출되어 새로운 인스턴스를 반환한다.<br>
<br>

```
@Configuration(proxyBeanMethods = false)
public class AppConfig {

    @Bean
    public ServiceA serviceA() {
        return new ServiceA(serviceB());
    }

    @Bean
    public ServiceB serviceB() {
        return new ServiceB();
    }
}
```


serviceA 빈을 생성할 때, serviceB() 메서드는 프록시를 통해 호출되지 않으므로 새로운 ServiceB 인스턴스가 생성된다. <br>
빈의 싱글톤 특성을 잃게 되어 필요에 따라 다르게 동작할 수 있다.<br>
<br>
<br>
<br>

### 결론
proxyBeanMethods = true (기본값) 은 일반적으로 빈 간의 순환 종속성을 해결하고 싱글톤을 보장하기 위해 사용된다.<br>

proxyBeanMethods = false은 빈 초기화가 단순하고 순환 종속성이 없으며, 성능 향상을 위해 프록시 오버헤드를 제거하려는 경우 사용한다. 
이 경우 빈의 순환 종속성을 해결할 수 없기 때문에, 종속성이 단순한 경우에만 사용해야한다.
