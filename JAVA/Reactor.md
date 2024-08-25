## Reactor

### Reactor 란
Reactor는 스프링 프레임워크 팀의 주도하에 개발된 리액티브 프로그래밍을 위한 라이브러리로, 리액티브 스트림즈 구현체 중 하나이다.  <br>
Spring Framework 5 버전부터 리액티브 스택에 포함되어 Spring webFlux 기반의 리액티브 애플리케이션을 제작하기 위한 핵심 역할을 담당한다. <br>

### Reactor 특징
Reacive Streams 사양을 구현한 리액티브 라이브러리로, jvm 위에서 실행되는 Non-Blocking 애플리케이션을 제작하기 위해 필요한 핵심기술이다. <br>
Mono, Flux 를 사용하여 Publisher 타입을 구분하며 Publisher 로부터 전달 받은 데이터를 처리하는데 과부하가 걸리지 않도록 제어하는 Backpressure 를 지원한다. <br>

Mono : 데이터를 1개 또는 1개만 emit 할 수 있는 reactor 의 publisher 타입이다. <br>
Flux : 데이터를 0개 또는 1개 이상 emit 할 수 있는 reactor 의 publisher 타입이다. <br>

```java
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

public class BasicReactorExample {

    public static void main(String[] args) {

        // 1. Flux: 여러 개의 데이터를 발행하는 Publisher
        Flux<String> fruitFlux = Flux.just("Apple", "Banana", "Orange", "Grape")   // just() Operatoreㄱ : reactor 에서 지원하는 Operator 메서드로, just() Operatore 는 데이터를 생성해서 제공하는 역할을 한다. 
                .map(String::toUpperCase) // 각 항목을 대문자로 변환
                .doOnNext(System.out::println); // 각 항목이 발행될 때마다 출력

        // 2. Mono: 단일 데이터를 발행하는 Publisher
        Mono<String> fruitMono = Mono.just("Pineapple")
                .map(String::toUpperCase) // 대문자로 변환
                .doOnNext(System.out::println); // 데이터가 발행될 때 출력

        // 3. Flux 구독 (데이터 처리 시작)
        fruitFlux.subscribe();

        // 4. Mono 구독 (데이터 처리 시작)
        fruitMono.subscribe();
    }
}
```
### Marble Diagram
마블 다이어그램 : https://projectreactor.io/docs/core/release/reference/index.html
<br>

### cold Sequence 와 Hot Sequence
cold 는 무언가를 새로 시작하고, Hot 은 무언가를 새로 시작하지 않는다. <br>
<br>

### backpressure
Publisher 가 끊임없이 emit 하는 무수히 많은 데이터를 적절하게 제어하여 데이터 처리에 과부하가 걸리지 않도록 제어하는 것이다.<br>
<br>

### sink 
Sinks 는 리액티브 스트림즈의 signal 을 프로그래밍 방식으로 푸시할 수 있는 구조로, Flux 나 Mono 의 의미 체계를 가진다. <br>
Flux 나 Mono 가 onNext 같은 Signal 을 내부적으로 전송해주는 방식이었는데, sinks 를 사용하면 프로그래밍 소스를 통해 명시적으로 signal 을 전송할 수 있다. <br>
Sinks 종류로는 Sinks.One 과 Sinks.Many 를 사용하는 방법이 있다.  <br>
한 건의 데이터를 전송하는 방법을 정의해 둔 기능 명세이다.  <br>
여러 건의 데이터를 여러가지 방식으로 전송하는 기능을 정의해 둔 기능 명세이다.  <br>
<br>

### backpressure / sink 예시 코드 
```java
import reactor.core.publisher.Flux;
import reactor.core.publisher.Sinks;
import reactor.core.scheduler.Schedulers;

public class ReactorExample {

    public static void main(String[] args) throws InterruptedException {
        
        // Sinks.many().multicast()를 사용하여 Flux Sink 생성
        Sinks.Many<Integer> sink = Sinks.many().multicast().onBackpressureBuffer();

        // Sink를 통해 방출되는 데이터 스트림 생성
        Flux<Integer> flux = sink.asFlux()
                .onBackpressureDrop(item -> System.out.println("Dropped: " + item)) // Backpressure 대응
                .publishOn(Schedulers.boundedElastic()) // 데이터를 처리할 스케줄러 지정
                .doOnNext(item -> {
                    try {
                        Thread.sleep(500); // 처리 지연 시뮬레이션
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    System.out.println("Processed: " + item);
                });

        // Flux 구독
        flux.subscribe();

        // Sink에 데이터를 방출
        for (int i = 0; i < 10; i++) {
            System.out.println("Emitting: " + i);
            sink.tryEmitNext(i);
        }

        // 프로그램이 종료되지 않도록 잠시 대기
        Thread.sleep(5000);
    }
}
```

### Scheduler
OS level 에서의 Scheduler 는 실행되는 프로그램인 프로세스를 선택하고 실행하는 등 프로세스의 라이프사이클을 관리해주는 관리자 역할이지만, <br>
Reactor 에서 사용되는 Scheduler 는 Reactor Sequence 에서 사용되는 스레드를 관리해주는 관리자 역할이다. <br>
Scheduler Operator 는 세가지가 있다. 
- subscribeOn() Operator : 구독이 발생한 직후 실행될 스레드를 지정하는 Operator 이다. 동시성을 가지는 논리적인 스레드에 해당한다. 
- publishOn() Operator : Downstream 으로 singal 을 전송할 떄 실행되는 스레드를 제어하는 역할을 하는 operator 이다. 동시성을 가지는 논리적인 스레드에 해당한다. 
- parallel() Operator : 라운드 로빈 방식으로 CPU 코어 개수만큼의 스레드를 병렬로 실행한다. 병렬성을 가지는 물리적인 스레드에 해당된다. 

물리적인 스레드 = 병렬성 = 실제로 동시에 실행되기 때문에 여러작업을 동시에 처리<br>
논리적인 스레드 = 동시성 = 동시에 실행되는 것처럼 보이도록 처리 <br>


```java
import reactor.core.publisher.Flux;
import reactor.core.scheduler.Schedulers;

public class ReactorSchedulerExample {

    public static void main(String[] args) throws InterruptedException {
        Flux.range(1, 10)
            .map(i -> {
                System.out.println("Mapping on thread: " + Thread.currentThread().getName());
                return i * 2;
            })
            .publishOn(Schedulers.parallel())  // 이후의 작업들을 parallel 스케줄러에서 실행하도록 지정. parallel 스케줄러는 기본적으로 스레드 풀을 사용하여 작업을 병렬로 수행
            .map(i -> {
                System.out.println("Mapping on parallel thread: " + Thread.currentThread().getName());
                return i + 1;
            })
            .subscribeOn(Schedulers.boundedElastic())  // 전체 시퀀스가 boundedElastic 스케줄러에서 시작되도록 설정. boundedElastic 스케줄러는 필요에 따라 스레드 풀을 확장하는 특징이 있음.
            .subscribe(i -> System.out.println("Received: " + i + " on thread: " + Thread.currentThread().getName())); // 최종적으로 데이터를 소비하며, 이 작업이 실행되는 스레드를 출력

        Thread.sleep(1000);
    }
}
```
- Scheduler.immediate() : 별도 스레드를 추가 생성없이 현재 스레드에서 작업을 처리한다. 
- Scheduler.single() : 스레드 하나만 생성해서 Scheduler 가 제거되기 전까지 재사용한다.
- Scheduler.boundedElastic() : Blocking I/O 작업에 최적화 되어있다. ExecutorService 기반의 스레드 풀을 생성한 후, 그 안에서 정해진 수만큼의 스레드를 사용하고 작업이 종료된 스레드는 반납하여 재사용한다.
- Scheduler.parallel() : Non-Blocking I/O 에 최적화되어 있는 Scheduler 로서 CPU 코어 수만큼의 스레드를 생성한다.  
- Scheduler.newSingle() / Scheduler.newBoundedElastic() / Scheduler.newParallel() : 메서드를 사용해서 새로운 Scheduler 인스턴스를 생성할 수 있다.


### Context
Reactor 의 Context 는 Operator 같은 Reactor 구성 요소 간에 전파되는 key / value 형태의 저장소이다. <br>
*전파는 Downstream 에서 upstream 으로 context 가 전파되어 Operator 체인 상의 각 Operator 가 해당 context 의 정보를 동일하게 이용할 수 있음을 의미한다. 
<br><br>
Reactor 의 Context 는 ThreadLocal 과 유사하지만 각각의 실행 스레드와 매핑되는 ThreadLocal 과 달리 Context는 Subscriber 와 매핑이 된다. <br>
즉, 구독이 발생할 때마다 해당 구독과 연결된 하나의 Context 가 생긴다. <br>

Context 에 데이터를 쓸 때는 Context 를 사용하며 (ex. ContextWriter() ), Context 데이터 읽는 방식은 두가지이다. 
1. 원본 데이터 소스 레벨에서 읽는 방식
   - Context 에 쓰인 데이터를 읽기 위해 deferContextual() Operator 사용한다. 
   - deferContextual() 는 Context 에 저장된 데이터와 원본 데이터 소스의 처리를 지연 시키는 역할을 한다.
   - 저장된 데이터를 읽을 때는 ContextView 를 사용한다.
2. Operator 체인의 중간에서 읽는 방식
   - transformDeferredContextual() Operator 를 사용한다.
   - Reactor 에서는 Operator 체인 상의 서로 다른 스레드 들이 Context 의 저장된 데이터에 손쉽게 접근할 수 있다. 

특징 
구독이 발생할 때마다 해당하는 하나의 Context 가 하나의 구독에 연결된다.<br>
Context 는 Operator 체인의 아래에서 위로 전파되기 때문에 일반적으로 contextWrite() 를 Operator 체인의 맨 마지막에 둔다. <br>
Context 는 인증 정보같은 독힙성을 가지는 정보를 전송하는데 적합하다. <br>


### Operators
Sequence 생성을 위한 operator 
- justOrEmpty : emit 할 데이터가 null 인 경우 nullPointException 대신 onComplete signal 을 전송
- fromIterable : Iterable 에 포함된 데이터를 emit 하는 Flux 를 생성
- fromStream : Stream 에 포함된 데이터를 emit 하는 Flux 를 생성함. 재사용이 안됨
- range : n 부터 1씩 증가한 연속된 수를 m 개 emit 하는 Flux 를 생성
- defer : 구독하는 시점에 데이터를 emit 하는 Flux 또는 Mono 를 생성 (just 는 hot publisher 여서 subscriber 의 구독 여부와 상관없이 데이터를 emit 하는데 defer 를 사용하면 구독이 발생하기 전까지 데이터의 emit 을 지연시키기 때문에 just operator 를 defer 로 감싸면 실제 구독이 발생해야 데이터를 emit 한다.)
- using : 파라미터로 전달받은 resource 를 emit 하는 Flux 를 생성
- generate : 프로그래밍 방식으로 signal 이벤트를 발생시키며 특히 동기적으로 데이터를 하나씩 순차적으로 emit 하고자 할 경우 사용

  



<br><br><br><br><br>

참고 <br>
Reactor 공식문서 : https://projectreactor.io/ <br>
https://projectreactor.io/docs/core/release/reference/index.html <br>
참고도서 : 스프링으로 시작하는 리액티브 프로그래밍_Spring WebFlux를 이용한 Non-Blocking 애플리케이션 구현 | 황정식 저자<br>
참고사이트 : https://www.baeldung.com/java-9-reactive-streams
<br>
