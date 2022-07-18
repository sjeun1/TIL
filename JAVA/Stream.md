

## Stream
<br>

### Stream 이란
c언어에서 스트림은 데이터 입출력 시 스트림에 바이트 문자들을 기록하여 스트림으로부터 데이터를 읽거나 출력하는 역할을 뜻하지만 
java SE8 에서의 스트림은 데이터를 추상화하여 다루며 다양한 방식으로 데이터를 읽고 쓰는 것이다.
<br><br>

### Stream 특징
-&nbsp;컬렉션(배열 포함)의 저장 요소를 하나씩 참조해서 람다식으로 처리할 수 있도록 해주는 반복자이다.<br>
-&nbsp;선언형으로 코드를 구현한다.<br>
-&nbsp;일회용이며, 원본 데이터를 읽기만 하고 데이터를 변경하지 않는다.<br>
-&nbsp;반복문이 내부에서 반복되어 코드 상에 노출되지 않는다.<br>
-&nbsp;filter, sorter, map, collect 등을 연결하여 데이터 처리 파이프라인을 만들 수 있다.<br>
<br>

#### Stream 적용 전, 후 예제
![image](https://user-images.githubusercontent.com/62210870/179526123-3555b675-3c36-4958-9dee-fe3d1122f413.png)
<br><br>

#### 컬렉션으로 Stream 얻기
![image](https://user-images.githubusercontent.com/62210870/179530439-b5a0ecd8-fca6-4f68-af80-c78ee598fb1d.png)
<br><br>

#### 배열으로 Stream 얻기
![image](https://user-images.githubusercontent.com/62210870/179530550-23c68852-7bae-4392-aed0-8940e26482de.png)
<br><br>

### 스트림 명령어
<br>

#### 중간연산 
<br>-&nbsp;filter() : 조건에 충족하는 요소 생성<br>
-&nbsp;distinct() : 중복 제거<br>
-&nbsp;sorted() : 정렬<br>
-&nbsp;mapToInt(),mapToLong(),mapToDobule() : map 타입이 숫자가 아닌 경우 변환<br>
<br><br>

#### 최종연산
-&nbsp;count() : stream 개수<br>
-&nbsp;sum() : 합<br>
-&nbsp;max(), min() : 최대,최소<br>
-&nbsp;allMatch() : 모두 일치하면 boolean 반환<br>
-&nbsp;anyMatch() : 하나라도 만족하면 boolean 반환<br>
-&nbsp;toArray() : 모든 값을 배열로 반환<br>
-&nbsp;findFirst() : 첫번째 값<br>
-&nbsp;findAny() : 랜덤 값<br>
<br><br>

#### Collector 연산 
Stream의 요소를 수집하여 요소를 그룹화 하거나 결과를 담아 반환하는데 사용.<br>
-&nbsp;Collectors.toList()<br>
-&nbsp;Collectors.toSet()<br>
-&nbsp;Collectors.toMap()<br>
-&nbsp;Collectors.groupingBy<br>
-&nbsp;Collectors.partioningBy<br>
-&nbsp;Collectors.summarizingInt()<br>
<br>
그 외 연산자는 https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html 에서 확인 가능하다.
<br><br><br>

## Lambda
<br>

### Lambda 란
메소드를 간단히 하나의 식으로 표현한 것으로 람다 함수는 함수형 프로그래밍 언어에서 사용되는 개념으로 익명 함수라고도 한다.<br>
불필요한 코드를 줄이고 가독성을 향상시키는 것을 목적으로 두고있다. 
( 파라미터 ) -> { 메소드내용 } 으로 구성되어 있다.
<br><br>

### Lambda 의 장점
-&nbsp;코드가 간결해져서 가독성이 높아진다.<br>
-&nbsp;병렬 프로그래밍에 용이하다.<br>
<br><br>

### Lambda 의 단점
-&nbsp;디버깅이 어렵다.<br>
-&nbsp;재사용이 불가능하다.<br>
<br><br>

#### Lambda 적용 전, 후 예제
![image](https://user-images.githubusercontent.com/62210870/179531459-16499f68-b186-4630-b1fa-5a13db33713b.png)
<br><br>

### 함수적 인터페이스(Functional interface) 란?
하나의 추상 메소드가 선언된 인터페이스만이 람다식을 사용할 수 있는데 이러한 인터페이스를 함수적 인터페이스라고 부른다.<br>
<br><br>

#### 함수적 인터페이스 중 IntFunction 예제
![image](https://user-images.githubusercontent.com/62210870/179541346-c4c2a700-f844-4fc7-8438-3c15ecd0be1a.png)
<br><br>

#### 함수적 인터페이스 중 BinaryOperator 예제
![image](https://user-images.githubusercontent.com/62210870/179541452-35ac8c0e-cf5c-41e2-86ad-7be8ae71fb1d.png)
<br><br>

그 외 다양한 람다 관련 자바 인터페이스는 해당 url 에서 확인이 가능하다. https://docs.oracle.com/javase/8/docs/api/index.html 

<br>
