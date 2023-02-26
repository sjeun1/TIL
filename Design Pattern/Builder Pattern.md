## Builder Pattern (정리중...)




사용 이유
1. 생성 시 지정할 인자 많을 때 사용
2. 객체 생성 시 여러 단계를 순차적으로 거칠 때, 이 단계의 순서를 결정해두고 각 단계를 다양하게 구현할 수 있도록 하는 패턴.


객체를 생성하는 클래스, 표현하는 클래스를 각각 분리하여 서로 다른 표현을 생성하는 방법을 제공.

생성자만 사용할 때 발생할 수 있는 문제를 개선하기 위해 만들어진 패턴이다.

Car Class 
실제로 생성하는 클래스
엔진, 에어백, 색상, 카메라센서 등 각 필드, 생성자, toString

Car Builder 
빌터 클래스
각 필드는 동일, Car build 메소드 추가 

Main 에 각 클래스를 이용해서 생성
Car car1 = new Car("V7", true, "Black", true, false);

메서드 체이님 ( 메서드 호출할 때 체인처럼 붙어있는 거)
build 메서드를 호출할 때 car 객체가 생성된다.
실제 객체 생성을 원하는 만큼 지연 할 수 있다.

추첨으로 에어백 장착 여부 결정할 때는 
Car car2 = new CarBuilder().setAEB(false)




---

builder 클래스는 무언가를 만들어 반환하는 
Director
Data
PlainTextBuilder
JSONBuilder 
XMLBuilder
