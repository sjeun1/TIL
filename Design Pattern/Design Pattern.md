## Design Pattern
디자인 패턴이란, 소프트웨어 디자인 패턴은 공통적으로 발생하는 문제에 대해 재사용 가능하고 효율적인 코드를 짜기 위해서 방법을 정의 한 것이다.
<br>

### Design Pattern 의 종류
생성(Creational), 구조(Structural), 행위(Behavioral) 패턴  총 3가지로 나뉜다. 

![image](https://user-images.githubusercontent.com/62210870/180816835-b22a79ae-bb92-42c4-bc7a-2615e39ab96d.png)
<br>

#### 생성 패턴(Creational Pattern)
-&nbsp;추상 팩토리(Abstract Factory) : 동일한 주제의 다른 팩토리를 묶어 준다.<br>
-&nbsp;빌더(Builder) : 생성(construction)과 표기(representation)를 분리해 복잡한 객체를 생성한다.<br>
-&nbsp;팩토리 메서드(Factory Method) : 객체를 생성하기 위해 인터페이스를 정의하고 하위클래스에서 팩토리 메서드를 오버라이딩해서 객체를 반환한다.<br>
-&nbsp;프로토타입(Prototype) : 기존 객체를 복제함으로써 객체를 생성함으로 시간, 자원 효율성을 높인다.<br>
-&nbsp;싱글턴(Singleton) : 한 클래스에 한 객체만 존재하도록 제한한다. (=하나의 인스턴스 보장)<br>
<br>

#### 구조 패턴(Structural Pattern)
-&nbsp;어댑터(Adapter) : 인터페이스가 호환되지 않는 클래스들을 함께 이용할 수 있도록, 타 클래스의 인터페이스를 기존 인터페이스에 덧씌운다.<br>
-&nbsp;브리지(Bridge) : 추상부와 구현부를 분리해 인터페이스와 구현의 결합도를 약화 시킨다.<br>
-&nbsp;합성(Composite) : 0개, 1개 혹은 그 이상의 객체를 묶어 하나의 객체로 이용할 수 있다.<br>
-&nbsp;데코레이터(Decorator) : 기존 객체의 매서드에 새로운 행동을 추가하거나 오버라이드 할 수 있다.<br>
-&nbsp;퍼사드(Facad) : 많은 양의 코드에 접근할 수 있는 단순한 하나의 인터페이스를 제공하고 느슨한 결합을 한다.<br>
-&nbsp;플라이웨이트(Flyweight) : 다수의 작은 객체들을 효과적으로 사용하여 생성·조작하는 비용을 절감할 수 있다.<br>
-&nbsp;프록시(Proxy) :  접근이 힘든 객체에 대해 본인 대신 그 일을 처리하여 접근 조절, 비용 절감,복잡도 감소를 시킨다.<br>
<br>

#### 행위 패턴(Behavioral Pattern)
-&nbsp;책임 연쇄 (Chain of Responsibility) : 각 객체들이 고리(Chain)로 묶여 있어 한 객체가 처리 못하면 요청이 해결될 때까지 고리를 따라 다음 객체로 책임이 넘어간다.<br>
-&nbsp;커맨드 (Command) : 요청을 객체의 형태로 캡슐화하여 재이용하거나 취소할 수 있도록 요청에 필요한 정보를 저장하거나 로그에 남기는 패턴이다.<br>
-&nbsp;인터프리터 (Interpreter) : 간단한 언어에 문법 표현을 정의하는 패턴이다.<br>
-&nbsp;반복자 (Iterator) :자료 구조와 같이 접근이 잦은 객체에 대해 동일한 인터페이스를 사용하고. 내부 표현 방법은 보여주지 않고 순화하여 데이터를 찾아가도록한다.<br>
-&nbsp;중재자 (Mediator) :수많은 객체들 간의 복잡한 상호작용(Interface)을 캡슐화하여 객체로 정의하는 패턴이다.<br>
-&nbsp;메멘토 (Memento) :특정 시점에서의 객체 내부 상태를 객체화함으로써 미리 저장해두었다가 요청에 따라 객체를 해당 시점의 상태로 돌릴 수 있는 기능을 제공한다.<br>
-&nbsp;옵서버 (Observer) :한 객체의 상태가 변화하면 객체에 상속되어 있는 다른 객체들에게 변화된 상태를 전달한다<br>
-&nbsp;상태 (State) :객체의 내부 상태에 따라 동일한 동작을 다르게 처리 할때 사용한다.<br>
-&nbsp;전략 (Strategy) :동일한 계열의 알고리즘들을 개별적으로 캡슐화하여 상호 교환할 수 있게한다.<br>
-&nbsp;템플릿 메소드(Template Method) :메소드에 정의하고 하위클래스에서 알고리즘 구조 변경 없이 알고리즘을 재정의한다.<br>
-&nbsp;방문자 (Visitor) :각 클래스들의 데이터 구조에서 처리 기능을 분리하여 별도의 클래스로 구성하는 패턴이다.<br>
-&nbsp;분리된 처리 기능은 각 클래스를 방문(Visit)하여 수행한다.<br>
<br>
