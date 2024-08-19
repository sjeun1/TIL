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
<br>
sink 예시 코드 
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


<br><br><br><br><br>

참고 <br>
Reactor 공식문서 : https://projectreactor.io/ <br>
https://projectreactor.io/docs/core/release/reference/index.html <br>
참고도서 : 스프링으로 시작하는 리액티브 프로그래밍_Spring WebFlux를 이용한 Non-Blocking 애플리케이션 구현 | 황정식 저자<br>
참고사이트 : https://www.baeldung.com/java-9-reactive-streams
<br>
