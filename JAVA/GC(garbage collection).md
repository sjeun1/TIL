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




참고 : https://d2.naver.com/helloworld/1329
https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%EC%85%98GC-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC

