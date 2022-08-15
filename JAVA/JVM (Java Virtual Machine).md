## JVM (Java Virtual Machine)
JVM 은 자바 어플리케이션을 OS 에 종속받지 않고 JVM이 깔린 어떤 종류의 컴퓨터에서 작동할 수 있도록 해주는 자바 가상 머신이다.<br>
<br>

### JVM 특징
OS 에 종속받지 않고 독립적으로 실행이 가능한 환경을 제공해준다.<br>
garbage collection 에 의해 메모리 관리를 알아서 해준다.<br>
<br>

### JVM 실행과정
1. .java 파일을 Java compiler 로 JVM 이 인식할 수 있는 자바 바이트코드(.class) 로 변환된다.<br>
&nbsp;&nbsp;(바이트코드 : jvm 에 이해할 수 있는 언어로 변환된 자바 소스 코드.*)<br>
&nbsp;&nbsp;(JDK 설치 시 bin 에 존재하는 javac.exe가 자바 컴파일러)<br>
쉘스크립트에 javac 파일명.java -> 같은 위치에 파일명.class 가 생성<br>
2. 바이트코드는 다시 실시간 번역기(인터프리터) 또는 JIT compiler 에 의해 바이너리 코드로 변환된다<br>
&nbsp;&nbsp;(바이너리코드 : 컴퓨터가 인식 할 수 있는 0과 1로 구성되는 이진코드)<br>
3. 로딩된 바이트 코드는 Execution Engine  을 통해 해석된다.<br>
4. 해석된 바이트 코드는 Runtime Data Area 에 배치되고 실행된다.<br>
<br>

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


