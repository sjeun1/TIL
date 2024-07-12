## JVM (Java Virtual Machine)
JVM 은 자바 어플리케이션을 OS 에 종속받지 않고 JVM이 깔린 어떤 종류의 컴퓨터에서 작동할 수 있도록 해주는 자바 가상 머신이다.<br>
<br>

### JVM 특징
OS 에 종속받지 않고 독립적으로 실행이 가능한 환경을 제공해준다.<br>
garbage collection 에 의해 메모리 관리를 알아서 해준다.<br>
<br>


### JVM 실행과정
1. JVM 은 OS로부터 메모리를 할당 받는다. 
2. java 파일을 Java compiler 로 JVM 이 인식할 수 있는 자바 바이트코드(.class) 로 변환된다.<br>
-&nbsp;&nbsp;(바이트코드 : jvm 에 이해할 수 있는 언어로 변환된 자바 소스 코드.*)<br>
-&nbsp;&nbsp;(JDK 설치 시 bin 에 존재하는 javac.exe가 자바 컴파일러)<br>
쉘스크립트에 javac 파일명.java -> 같은 위치에 파일명.class 가 생성<br>

3. Class Loader 는 변환된 클래스파일의 바이트 코드를 읽어 Rumtime Data Area 에 올린다. <br>

*&nbsp;Class Loader <br>
클래스로더는 위임모델, 계층구조의 특징이 있다. <br>
위임모델은 자신에게 클래스 로딩 요청이 들어오면 자신의 부모 클래스 로더에게 요청을 보내고, <br>
부모가 못찾으면 자기가 클래스를 탐색하는 것이다 <br>
계층구조는 상위 클래스로더의 클래스는 하위클래스에서 볼 수 있다는 것이다. (그 반대는 불가능) <br>

-&nbsp; Class Loader 는 총 3가지로 부트스트랩 클래스 로더, 플랫폼 클래스 로더, 시스템 클래스 로더로 구분된다. <br>
-&nbsp; Bootstarp Class Loader : jvm 이 시작될 때 실행되는 jvm 에 내장된 네이티브 코드로, 실행에 필요한 클래스를 로딩한다. <br>
-&nbsp; Platform Class Loader : Bootstrap Class Loader 를 부모로 가지고 있는 인스턴스로 자바에서 기본적으로 제공해주는 클래스를 로딩할 때 사용된다. <br>
-&nbsp; System Class Loader : 유저가 작성한 클래스를 로딩할 때 사용되는 인스턴스이며 Platform Class Loader 를 부모로 가지고 있다. ClassPath 에 명시된 경로를 통해 클래스를 찾는다. <br>
<br>

*&nbsp;Runtime Data Area <br>
runtime data area 는 os 로부터 할당받은 메모리 영역을 말하며 총 다섯개의 영역으로 이루어져있다. <br>
-&nbsp; Method Area : 모든 쓰레드가 공유되며 클래스에 대한 정보가 올라온다. <br>
-&nbsp; Heap : 모든 쓰레드가 공유되며, 인스턴스가 생성되는 영역<br>
-&nbsp; Stack : 지역변수, 매개변수, 리턴값 등의 값이 저장되는 영역<br>
-&nbsp; PC Register : 현재 쓰레드가 실행되는 부분의 주소와 명령을 저장하는 영역<br>
-&nbsp; Native Method Stack : Native 언어로 작성된 코드를 실행하기 위한 영역<br>
<br>

4. Runtime Data Area 에 로딩된 바이트 코드는 Execution Engine 을 통해 해석된다. <br>
&nbsp;여기서 Exection Engine 에 의해 GC 작동 및 Thread 동기화가 이루어진다. <br>

5. 해석된 바이트코드는 Runtime Data Area 에 배치되고 실행된다. <br><br>

### JVM 의 구성요소
1. 클래스 로더(Class Loader) : 바이트코드를 필요한 순간에 JVM 으로 로딩<br>
2. 실행엔진 (Exceution Engine) : 로드된 클래스 파일의 바이트코드를 실행하는 엔진<br>
-&nbsp;인터프리터(Interpreter) : 명령어를 한줄씩 해석하여 실행하는 역할을 수행한다. <br>
-&nbsp;JIT 컴파일러(Just-in-Time Compiler) : 인터프리터 방식의 단점을 보완하기 위해 도입된 컴파일러이다.<br>
3. 가비지 콜렉터(Garbage collector) : 메모리 관리를 자동으로 해준다.<br>
4. 런타임 데이터 영역 (Runtime Data Area)<br>
-&nbsp;Method Area (모든 스레드가 공유) <br>
-&nbsp;Method 영역은 JVM 시작될 때 생성되는 공간이다. 바이트코드는 이영역에 저장이 된다.<br>
-&nbsp;Heap (모든스레드가 공유) : 동적으로 생성된 객체(ex. 인스턴스 변수)가 저장되는 영역으로 GC의 대상이 되는 영역이다<br>
-&nbsp;Stack : 지역변수, 메서드 매개변수 등 임시적으로 사용되는 변수가 저장되는 영역이다. <br>
-&nbsp;PC Register : 현재 수행중인  jvm 의 명령어 주소를 저장하는 공간이다.<br>
-&nbsp;Netive Method Stack : Java가 아닌 다른 언어로 작성된 코드를 위한 공간이다.<br>
<br>


