

## Stack
스택은 데이터를 임의 저장할 수 있는 자료구조로 값을 한쪽에서만 가능한 후입선출(LIFO, Last-In-First-Out) 방식을 가지고 있다.
![image](https://user-images.githubusercontent.com/62210870/182622365-20815091-0fcb-4252-83e8-10713d5666d5.png)
<br>

### Stack 의 장점
구조가 단순한 편으로 구현이 쉽다. 
데이터 저장/읽기 등 접근 속도가 빠르다.
<br>

### Stack 의 단점
데이터 최대 갯수를 미리 정해야한다.
데이터 삽입과 삭제가 효율적이지않다.
<br>

### Stack 사용 예시
인터넷에서 뒤로가기 버튼 - 스택으로 구현된 메소드
<br>

### Stack 예제

![image](https://user-images.githubusercontent.com/62210870/182639658-54c1c063-f354-40a9-8675-30a3964ee013.png)

<br>
-&nbsp;push(data):  스택의 가장 윗 부분에 data를 추가한다.
-&nbsp;pop(): 스택에서 가장 위에 있는 항목을 제거한다.
-&nbsp;peek(): 스택의 가장 위에 있는 항목을 조회한다
-&nbsp;isEmpty(): 스택이 비어 있을 때에는 true, 아닐때는 false 를 반환한다.
-&nbsp;clear() : 스택을 모두 비우고 초기화한다

위가 기본적인 예제이고 보통 스택을 구현할 때에는 배열을 이용하는 방법과 연결리스트로 구현하는 방법이 있다.
