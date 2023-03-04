# DI (간단메모)

생성자 주입과 필드 주입 (@Autowired) <br>

객체 불변성 -> 변경 가능성 배제하여 불변성 확보. <br>
테스트 코드작성 -> 주입 객체 누락 시 컴파일 시점에 오류 발견 가능<br>
final 키워드 작성 -> 컴파일 시점에 누락된 의존성 확인<br>
순환 참조 에러 방지 -> 서로참조시 callStack 이 쌓여 stackOverflow<br>
<br>

<br>
1. 
@Autowired
private ARepository aRepository;  // inject 할 때는 final x

2. 
public AController(ARepository aRepository) {
	this.ARepository = aRepository;
}

3.
@Autowired
public void setA(ARepository aRepository) {
	this.A = A;
}

<br><br><br>
스프링ioc 컨테이너가 AController 인스턴스를 만들어주고나서 setter 를 통해서 ioc container 에 들어있는 bean 중에 ARepository 타입을 찾아서 넣어준다.<br>
<br>
스프링 프레임워크에서 권장하는 방법은 생성자생성자 주입하는게 좋은 이유는 필수적으로 사용해야하는 레퍼런스없이는 AController 인스턴스를 만들지 못하도록 강제할 수 있다.<br>
<br>
