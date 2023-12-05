## Compile 과 implementation 



Compile <br>
특정 모듈을 수정 시 해당 모듈에 의존하는 모든 모듈이 rebuild 된다. <br>
연결된 모든 모듈의 api 가 노출된다 <br>
<br>

Implementation <br>
특정 모듈을 수정하게 되면 직접적으로 의존하는 모듈까지만 rebuild 하여 속도가 더 빠르다<br>
사용자에게 api 노출되는 것을 막는다.<br>
<br>

** 결론 : Gradle 최근 버전 부터는 compile 이 deprecated 되서 implementation을 를 사용해야한다.<br>
