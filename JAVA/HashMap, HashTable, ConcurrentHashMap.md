## HashMap, HashTable, ConcurrentHashMap
모두 Map 인터페이스를 구현하고 key, value 구조로 되어있다. <br>

### HashMap
java.util 패키지 안에 Map 인터페이스를 구현한 HashMap 을 사용한다. <br>
null 값을 넣을 수 있다. null 에 대해 예외처리를 하지않는다. <br>
모든 메서드에 synchronized 키워드가 붙어있지 않다. <br>
Lock을 걸지 않아 성능은 좋지만 멀티스레드 환경에서는 문제가 발생할 수 있다. <br>
<br>

### HashTable
java.util 패키지 안에 Map 인터페이스를 구현한 HashTable 을 사용한다. <br>
각 key 값에 해시함수를 적용해서 배열의 고유한 index 를 생성하고 값을 저장, 검색할 때 사용한다. <br>
put 메서드는 null 값을 허용하지 않고 있다. <br>
모든 메서드에 synchronized 키워드가 있어서 thread-safe 하다는 특징이 있지만 요청마다 Lock 을 걸기 때문에 병목현상이 발생할 수 있다. <br>
<br>

### ConcurrentHashMap
java.util.conccurrent 패키지 안에 Map 인터페이스를 구현한 ConcurrentHashMap을 사용할 수 있다.<br>
위의 HashTable 클래스의 단점을 보완하기위해 JDK 1.5에 등장한 클래스로 멀티 스레드 환경에서 사용한다.<br>
모든 메서드에 synchronized가 붙어있진 않고 일부에만 Lock을 걸어서 좀 더 성능 오버헤드를 개선시킨다. <br>
<br>
<br>
<br>


```
