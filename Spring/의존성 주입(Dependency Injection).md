## DI (Dependency Injection)

의존성 주입이란, 제어의 역전이 일어날 때 스프링이 내부에 있는 객체들 간의 관계를 관리할 때 사용하는 방법이다. <br>
특정 객체에 필요한 객체를 외부에서 결정해서 연결 시키는 것을 의미한다. <br>
의존성 주입을 통해 모듈간의 결합도를 낮추고 유연성을 높일 수 있다. <br>
<br>


생성자 주입과 필드 주입 (@Autowired) <br>

객체 불변성 -> 변경 가능성 배제하여 불변성 확보. <br>
테스트 코드작성 -> 주입 객체 누락 시 컴파일 시점에 오류 발견 가능<br>
final 키워드 작성 -> 컴파일 시점에 누락된 의존성 확인<br>
순환 참조 에러 방지 -> 서로참조시 callStack 이 쌓여 stackOverflow<br>
<br>

<br>
1. 

```
@Autowired
private ARepository aRepository;  // inject 할 때는 final x
```

2.
```
public AController(ARepository aRepository) {
	this.ARepository = aRepository;
}
```

3.
```
@Autowired
public void setA(ARepository aRepository) {
	this.A = A;
}
```

<br><br><br>
스프링ioc 컨테이너가 AController 인스턴스를 만들어주고나서 setter 를 통해서 ioc container 에 들어있는 bean 중에 ARepository 타입을 찾아서 넣어준다.<br>
<br>
스프링 프레임워크에서 권장하는 방법은 생성자생성자 주입하는게 좋은 이유는 필수적으로 사용해야하는 레퍼런스없이는 AController 인스턴스를 만들지 못하도록 강제할 수 있다.<br>
<br>


### annotation 별 생성자 생성
NoArgsConstructor -> 디폴트 생성자 <br>
AllArgsConstructor -> 모든 속성 포함하는 생성자 <br>
RequiredArgsConstructor -> @NonNull 이나 final 키워드를 사용하고 있는 속성들의 생성자 <br>
<br>

