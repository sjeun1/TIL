
## AOP

oop 를 보완하는 수단으로 흩어진 aspect 를 모듈화 할 수 있는 프로그래밍 기법.

- Aspect : 모듈화 한 것을 의미, aspect 안에는 advice 랑 point cut 이 있다. 
- Target : 적용이 되는 대상 (클래스 들)
- Advice : 해야할 일들
- Join point : 메서드 실행할 때 적용하는 시점(생성자 호출 직전, 필드 접근 전, 필드에서 값을 가져갔을 때)
- Point cut : 어디에 적용해야 하는지

### AOP 적용 방법
- 컴파일
- 로드파일
- 런타임
 
aop 구현체는 자바에서는 aspectJ, 스프링 aop 가 있다.
