## Singleton Pattern
싱글톤 패턴이란, 애플리케이션이 시작될 때 한 클래스가 최초로 한 번 메모리를 할당하고 메모리에 인스턴스를 만들어 사용하는 패턴이다.<br>
생성자가 여러번 호출되어도 실제 생성되는 객체는 하나이고 최초 생성 이후 호출된 생성자는 기존 생성된 객체를 반환한다.<br>
<br>

### Singleton Pattern 의 장점
객체를 생성할 때마다 메모리 영역을 할당받는데 한번의 new 를 통해 객체를 생성하면 메모리 낭비를 방지한다. <br>
전역 인스턴스이므로 다른 클래스간에 데이터를 공유하는 것이 가능하다.<br>
<br>

### Singleton Pattern 단점
너무 많이 엮여있으면 결합도가 높아져 OOP 의 개방-폐쇄 원칙에 위배된다<br>
멀티스레드 환경에서 동기화 처리가 안되어있으면 인스턴스가 1개 이상이 생성되어 동시성 문제가 생길 수 있다<br>
(*동시성문제 : 여러개의 스레그가 사용되어 CPU Cache Memory와 RAM 의 데이터가 불일치하여 생기는 문제*)<br>
<br>

### Singleton Pattern 사용법

#### 1. 기본 예제

![image](https://user-images.githubusercontent.com/62210870/181779339-9944f5ca-3b3b-4230-8e42-7b75c51c6afe.png)

해당 예제는 외부에서 호출 못하게 생성자를 private 으로 설정하고, public 으로 설정된 getInstance 만 호출하는데 <br>
instance 가 null 일 때(최초로 설정되는 때) 만 생성하는 코드이다.<br>
이 예제는 가장 기본적인 사용 방법이지만 thread safety 하지못한 코드이다<br>
(*thread safety : 다른 함수가 해당 함수를 호출 할 때 다른 스레드가 호출하여도 올바른 값이 응답되는 것*)<br>
이러한 문제를 해결하기 위해서 Synchronzied 를 사용하여 코드를 작성한다<br>
<br>

#### 2. Synchronized 예제

![image](https://user-images.githubusercontent.com/62210870/181781184-26fe93f5-e7b4-4781-a8fe-907c7f7926e9.png)

해당 예제는 앞서 확인한 첫번째 예제에서 Synchronized 를 적용한 예제이다.<br>
하지만 위의 단점을 방지하고자 synchronized 를 지나치게 많이 사용하게 된다면 병목현상을 겪을 수도 있다.<br>
<br>

#### 3. Double Checked Locking(DCL) 예제

![image](https://user-images.githubusercontent.com/62210870/181785926-4288ae17-ae06-4bac-80d6-9cc810910242.png)

해당 예제는 null 일 경우에만 synchronized 블록을 체크하는 코드로 병목현상을 방지 할 수 있다<br>
여기에서는 volatile 를 확인 할 수 있는데 volatile 는 Main Memory 에서 CPU의 값을 읽거나 쓰기때문에 주로 여러 스레드가 동시에 접근할 수 있는 변수를 volatile 로 선언한다.<br>
(하나의 쓰레드만 읽고 쓰고, 나머지 스레드가 읽는 상황에서 가장 최신 값을 보장한다)<br>
그런데 메인 메모리로부터 데이터를 읽고 쓰는 작업은 CPU 캐시를 이용하는 것 보다 많은 비용이 요구되며 volatile 선언은 JVM 의 성능 향상을 위한 기술인, 코드 재정리를 막기도 하는 등의 문제가 있을 수 있다고 한다. <br>
(volatile 참고1 : https://www.hanbit.co.kr/channel/category/category_view.html?cms_code=CMS6818849791 )<br>
(volatile 참고2 : https://jenkov.com/tutorials/java-concurrency/volatile.html )<br>
<br>

#### 4. LazyHolder

![image](https://user-images.githubusercontent.com/62210870/181800238-c15aeabb-204b-4b00-9a79-de4aea072ecc.png)

해당 예제는 singleton pattern 을 사용할 때 주로 사용되는 예제로, 생성될 객체를 Lazy Loading 하는 코드이다.<br>
클래스가 로딩될때 LazyHolder 라는 내부 클래스는 로딩되지 않고 getInstance 를 호출할 때 클래스가 로딩되어 필요할 때만 호출되도록 하는 것이다.<br>
해당 코드는 위에서 사용한 synchronized 등을 사용하지 않고도 멀티스레드 환경에서도 사용할 수 있다.<br>
왜냐하면 JVM 에서 클래스를 로딩하는 과정에서 멀티 스레드 환경에서도 안전한 동기화 환경을 제공하기 때문이다.<br>
<br>



