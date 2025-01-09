# GC (garbage collection)
<br>

-&nbsp;JVM 의 Heap 영역에서 동적으로 할당했던 메모리 중 필요없는 메모리 객체를 제거 하는 프로세스.<br>
개발자가 메모리 관리, 메모리 누수(Memory Leak) 문제 관리를 안해도 된다. <br>
어떤 필요없는 객체로 판별하는지 궁금해서 찾아보니 일반적으로 JVM 메모리에서는 객체들은 Heap 에서 생성, Method Area 나 Stack Area 에서는 Heap 에서 생성된 객체를 주소만 참조하는데 heap 객체에 레퍼런스가 있으면 참조된 상태, 없으면 GC 의 대상이 된다고 한다. <br>
<br>

프로그램이 실행되면 OS 는 메모리 공간을 할당해주는데 주로 메인 메모리(RAM) 에 할당되며, <br>
메모리 공간은 스택, 힙, 데이터 영역으로 나누어지게 된다.<br>
GC 의 단점은 메모리가 언제 해제되는 지 정확하게 알 수 없어 제어가 힘들고, STW 로 인해 오버헤드가 발생한다. <br>
**STW(Stop-The-World)** : GC 가 동작하는 동안 다른 동작을 멈추는 현상. GC 관련 Thread 제외한 모든 Thread 가 멈춤<br>
개발자는 어플리케이션의 사용성을 유지하면서 효율적이게 GC 를 실행하는 최적화 작업, GC 튜닝을 해야한다.<br>
<br>

#### Heap 영역에 들어가는 데이터
-&nbsp;힙 영역에는 주로 배열과 같은 동적으로 생성된 데이터가 들어간다. <br>
-&nbsp;New 키워드를 통해 생성된 객체들<br>
-&nbsp;배열<br>
-&nbsp;클래스 멤버 중 참조 타입 (String, 생성된 객체 타입)<br>
-&nbsp;상수 풀 : 상수풀이란 리터럴 값 or 상수값 을 효율적으로 관리하기 위한 메모리 영역 이고 두가지로 나뉜다.<br>
&nbsp;&nbsp;1)클래스 상수풀 <br>
&nbsp;&nbsp;&nbsp;컴파일 된 클래스 파일 내부에 포함 <br>
&nbsp;&nbsp;&nbsp;클래스의 메서드 필드, 인터페이스 이름을 저장하여 런타임에 사용되도록 함<br>

&nbsp;&nbsp;2)런타임 상수풀<br>
&nbsp;&nbsp;&nbsp;JVM 이 클래스를 로딩할 때 클래스 상수 풀의 값을 가져와 생성되는 상수 풀.<br>
&nbsp;&nbsp;&nbsp;자주 사용되는 값을 관리하며, 문자열 리터럴과 정수형 상수 등이 주로 저장됨<br>
<br>

#### Heap 메모리 구조
Heap 메모리의 구조는 Young과 Old 총 3가지 영역으로 되어있다. <br>
**-&nbsp;Young 영역** : 새롭게 생성된 객체가 할당.<br>
**-&nbsp;Old 영역** :  young 영역에서 오래 남아 있는 객체들. young 영역보다 크게 할당됨<br>
**-&nbsp;Perm 영역** : 없는 영역으로 넘겨도됨. 자바 언어 레벨에서 사용하는 영역이 아님, JDK8 에서는 사라짐<br>
<br>

#### Young 영역
young 영역은 효율적인 GC 를 위해 Eden, survivor0, suvivor1로 나눠진다<br>
**-&nbsp;Eden** : new 를 통해 새로 생성된 객체가 위치<br>
**-&nbsp;survivor0,1** : 최소 1번의 GC 이상 살아남은 객체가 존재.<br>

-> survivor 영역중 1개는 반드시 사용되어야하고 하나는 반드시 비워져있는 상태여야하는 제한 조건이 존재. <br>
&nbsp;&nbsp;(두영역에 모두 존재하거나 모두 0이면 비정상)<br>
-> survivor 에서 살아남을 때마다 살아남은 횟수 체크하는 age 가 1씩 증가.<br>
<br>

#### young 영역에서의 동작 방식
새로운 객체가 Eden 영역 생성-> Eden 영역이 다차면 GC 가 동작(Minor GC) -> 살아남은 객체가 Survivor0 이동-> survivor0 이 꽉차면 0에서 GC 동작-> 살아남은 객체들 survivor1로 이동-> 살아남은 객체 Heap 의 Old 로 이동-> Old 영역이 다차면 Old 영역에 대한 GC (Major GC 또는 Full GC) 가 실행이 일반적인 동작방식이다.<br>
그런데 Survivor 영역을 거치지 않고 바로 old 영역으로 이동하는 객체가 있을 수 있는데, 해당 경우는 객체의 크기가 아주 큰 경우에는 바로 old 영역으로 이동하게 된다.<br>
Young 영역에서 발생하는 GC 를 마이너GC, old나 perm 영역에서 발생하는 GC를 메이저GC 라고 한다. <br>
Minor GC 는 빠른데 Major GC 는 느려서 여기서 STW가 발생한다. <br>
<br>
<br>
#### 5가지 GC 방식
-&nbsp;Serial GC : 싱글 스레드로 동작하는 GC 방식이다. 그만큼 느리고 STW 시간도 다른 GC에 비해 길다. 보통 실무에서 사용하는 경우는 없고 다만 CPU가 1코어인 경우에만 사용된다. <br> 
-&nbsp;Parallel GC : Java 8에서 Default GC 로 Serial GC보다 STW 시간이 훨씬 짧다. Parallel GC는 Minor GC를 처리하는 스레드를 여러 개로 늘려 병렬로 처리하여 Serial GC보다 훨씬 빠르게 동작하는 방식이다. ( Old 영역 X)
<br>
-&nbsp;Parallel Old GC (Parallel Compacting Collector) : Parallel Old GC는 Old 영역까지 멀티스레드 방식을 사용한다.<br>
-&nbsp;CMS GC (Conccurrent Mark-Sweep Collector) : Stop The World로 자바 어플리케이션이 멈추는 현상을 줄이고자 만든 GC <br>
-&nbsp;G1 GC(Garbage First Collector) : Java 9+ 에서 Default GC (Oracle JDK 7 update 4 이상 릴리즈에서 지원은 가능). 현재 GC 중 STW의 시간이 제일 짧다. 앞에 나왔던 GC들과는 다르게 Eden, Survivor, Old 영역이 고정된 크기가 아니며 전체 힙 메모리 영역을 Region이라는 특정한 크기로 나눈다.
Region의 상태에 따라 그 Region의 역할(Eden, Survivor, Old)가 동적으로 변동한다.
<br><br>

### GC 의 성능을 높이는 방법 
#### 1. 설정을 변경하여 GC 의 성능을 높이는 방법<br>
-&nbsp;애플리케이션을 중단한 후 GC 병렬로 동시 진행<br>
-&nbsp;애플리케이션과 GC 작업을 동시 진행<br>

#### 2. 코드를 변경하여 GC 의 성능을 높이는 방법<br>
-&nbsp;Collection 등을 활용할 때 사용할 객체의 크기를 명시.<br>
-&nbsp;불변 객체 사용 (원시타입일 경우는 final 을 사용해서 setter 못하게 하는 방식)   --------> [원시타입, 참조타입](https://github.com/sjeun1/TIL/blob/main/JAVA/%EC%9B%90%EC%8B%9C%ED%83%80%EC%9E%85%EA%B3%BC%20%EC%B0%B8%EC%A1%B0%ED%83%80%EC%9E%85.md)
<br>

```
public class MutableHolder {
  private Object value;
  public Object getValue() { return value; }
  public void setValue(Object o) { value = o; }
}
```

다른 값을 참조하여 생존. old 영역으로 이동하게 됨. <br>
old 영역에서도 참조하는 객체가 바뀌고, minor GC 수행 시 old 영역으로 와서 mutableHoler 검사해서 young 영역 정리가 필요하다.<br>
<br>

```
public class ImmutableHolder {
  private final Object value;
  public ImmutableHolder(Object o) { value = o; }
  public Object getValue() { return value; }
}
```

ImmutableHolder 를 사용하면 참조하는 값이 먼저 존재해야하고, old 영역으로 이동하게 되면 <br>
MutableHolder Minor GC에 대한 검사를 하지 않아도 되므로, 스캔의 범위를 줄여 성능을 높일 수 있음
<br>
* 그 외에도 side Effect 방지, Thread-Safe 하여 병렬 프로그래밍 유용(동기화고려X) 등의 이유로 불변객체, final 을 사용하는게 좋음 (이펙티브자바에도 관련내용 나옴)<br>
<br>

++ 내용 추가 <br>

#### 2. 코드를 변경하여 GC 의 성능을 높이는 방법 요약
메모리 누수를 최소화하고, 불필요한 객체 생성을 줄인다. <br>
컬렉션 초기화 시 크기를 지정한다. <br>
컬렉션 사용 후 명시적으로 clear() 호출한다. <br>
지역변수 사용 후 참조 제거한다. (Ex. 참조를 null 로 설정하여 gc 대상이 되도록 함)<br>
parallelStream() 은 무분별하게 사용하면 많은 스레드가 생기므로  필요한 경우에만 사용한다.<br>
IO, 네트워크 등 리소스를 다룰 때 try-with-resources 를 사용하여 불필요한 객체 참조를 제거한다. <br>
Immutable Object 를 사용한다. (ex. 필드에 final 사용하고 setter 구현X)<br>
빈번하게 생성되고 버려지는 객체는 객체 풀을 통해 재사용하도록 설계한다. <br>


## GC 튜닝 및 JVM 최적화
1. GC 알고리즘 선택
애플리케이션 특성에 따라 적합한 GC 알고리즘을 선택한다. (ex. -XX:+UseG1GC ) <br>
- Serial GC: 단일 스레드, 작은 애플리케이션에 적합.
- Parallel GC: 여러 스레드로 GC 수행, CPU 효율 높음.
- G1 GC: 대규모 서비스, 예측 가능한 STW 시간.
- ZGC, Shenandoah: 지연 시간(Latency) 최소화, 최신 GC 알고리즘.

2. Heap 크기 및 메모리 설정
스택 메모리는 JVM 이 자동으로 관리하고 메서드 종료 시 자동으로 해제되어 일반적으로 잘하진 않는다. <br>
하지만 힙은 관리가 필요하고 일반적으로 -Xms, -Xmx 를 둘 다 설정해야한다. <br>
동일하게 설정하면 동적 확장/축소를 방지하여 성능을 안정시킬 수 있다. <br>
적절히 큰 -Xmx 값을 설정하면 OutOfMemoryError 를 발지 할 수 있다. <br>
적절히 큰 -Xms 값을 설정하면 애플리케이션 실행 초기 메모리 부족으로 인한 확장을 방지할 수 있다. <br>

- 어떻게 결정하는가? <br>
  - 애플리케이션의 메모리 요구량<br>
  - 애플리케이션의 트래픽<br>
  - 가비지 컬렉션의 동작 <br>

주의 <br>
너무 작게 설정하는 경우 : 자주 GC가 실행되고 성능이 저하 <br>
너무 크게 설정하는 경우 : 시스템 전체 메모리가 부족해지고, 성능이 악화되거나 OutOfMemoryError가 발생 (리소스 낭비) <br>

주의 : JVM 이 초기 힙 메모리 부족으로 인해 힙을 확장해야하는 경우, 확장 작업은 CPU 오버헤드가 발생하고 성능이 저하될 수 있다. <br>
```
-Xms512m -Xmx1024m -XX:NewRatio=2

// Xmx1024m : JVM 이 사용할 수 있는 최대 힙 메모리 크기
// Xms512m : JVM 이 시작될 때 할당할 초기 힙 메모리 크기
```

3. GC 로그 분석
프로파일링 도구를 사용하여 애플리케이션의 메모리, CPU 사용량, GC 동작을 분석. (JProfiler, VisualVM, Eclipse MAT.)  <br>
애플리케이션 모니터링 도구를 사용하여 CPU 및 메모리 사용량을 실시간으로 모니터링. (Prometheus, Grafana) <br>
GC 로그 분석하여 GC 빈도, STW 시간을 확인 (ex. -XX:+PrintGCDetails -Xloggc:gc.log) <br>

 <br> <br> <br> <br>

참고 : https://d2.naver.com/helloworld/1329
https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%EC%85%98GC-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC

