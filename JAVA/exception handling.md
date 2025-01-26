# exception handling

에러(error) 란 시스템 레벨에서 발생하는 치명적인 문제 를 말하며 개발자가 코드로 해결할 수 없는 문제를 의미한다. <br>
대부분의 에러는 JVM 에서 발생하며 메모리 부족(outOfMemory) , 스택오버플로우(stackOverflowError) 와 같은 상황에서 발생한다. <br> 
개발자는 이러한 에러를 예방하기 위해 [메모리 효율적인 코드 작성](https://github.com/sjeun1/TIL/tree/main/JAVA), 재귀 호출 제한하는 방식 등을 사용한다. <br>

예외(exception) 는 프로그램 실행 중 발생할 수 있는 예외 조건을 말하며 코드 상으로 처리가 가능하다. <br>

크게 2가지로 checked exceptions 과 unchecked exceptions 으로 나뉜다. <br>
checked exceptions 은 주로 IOException 등이 있다. <br>
unckecked excetions 으로는 nullPointExcetpion, arrayIndexOutBoundsException 등 이 있다.<br>

발생 시점에 따른 에러 분류가 필요하다. compile error 인지, runtime error 인지, logical error 인지... <br>
compile error : 컴파일 시점의 오류로, 코드의 오타나 syntax 오류, 자료형 체크 등에서 발생하는 에러를 말한다. <br>
runtime error : 프로그램 실행 시점의 오류로, 실행 도중 의도치 않은 동작에 의해 발생하는 에러를 말한다. 대표적인 unckecked excetions 이다. <br>
logical error : 다른 오류처럼 시스템을 멈추진 않지만 개발자 의도와 다르게 동작하는 에러를 말한다. <br>
