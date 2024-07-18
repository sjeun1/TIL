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

### Java에서 HashTable 클래스는 ConcurrentHashMap로 대체되었는데 HashTable이 deprecated 되지 않은 이유
1. Backward Compatibility (하위 호환성) <br>
Java는 오래된 코드가 새로운 버전에서도 작동하도록 하위 호환성을 중요시한다. 많은 기존 코드와 라이브러리가 HashTable을 사용하고 있기 때문에 이를 deprecated로 만들면 기존 코드가 경고 메시지를 띄우거나 향후 버전에서 작동하지 않을 위험이 있다.
2. Legacy Code Support (레거시 코드 지원)<br>
HashTable은 초기 Java 버전부터 존재한 클래스여서 많은 레거시 시스템이 여전히 HashTable을 사용하고 있으며, 이를 deprecated로 만들면 이런 시스템의 유지 보수와 호환성에 문제가 생길 수 있다.
3. 기능 차이 <br>
ConcurrentHashMap은 멀티스레드 환경에서의 성능과 확장성을 고려하여 설계되었으며, HashTable보다 효율적이다. 그러나 HashTable은 여전히 동기화된 맵을 제공하며, 간단한 동기화가 필요한 상황에서는 여전히 유용할 수 있다.
4. API 안정성 <br>
Java의 설계 철학 중 하나는 API의 안정성을 유지하는 것인데 이미 널리 사용되고 있는 클래스를 deprecated로 만들면 API 사용에 혼란을 줄 수 있다. 대신, Java는 새로운 클래스를 도입하여 더 나은 대안을 제공하는 방식을 선호한다.
<br>

이러한 이유들로 인해 HashTable은 deprecated로 지정되지 않았지만, <br>
새로운 코드에서는 ConcurrentHashMap이나 다른 최신 동기화 맵을 사용하는 것이 권장된다. <br>
ConcurrentHashMap은 성능과 확장성 면에서 더 우수하며, 최신 Java 애플리케이션 개발에 더 적합하다.<br>
<br>
<br>


```
