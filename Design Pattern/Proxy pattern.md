
## Proxy pattern

프록시패턴이란, 객체 지향 디자인 패턴 중 하나로 제어 흐름을 조정하기 위한 목적으로 중간에 대리자를 두어 대신처리하는 패턴이다. <br>
클라이언트가 실제 객체에 직접 접근하는 대신 프록시 객체를 통해 간접적으로 접근한다. <br>
직접호출 : client->server<br>
간접호출 : client->proxy->server<br>
프록시 객체는 실제 객체와 같은 인터페이스를 구현하며, 클라이언트는 프록시를 통해 실제 객체에 접근한다. <br>
<br>

### 간접 접근하는 이유? <br>
-대상 클래스가 민감한 정보를 가지고 있는 경우(보안 이슈) <br>
-인스턴스화 하기에는 인스턴스화 과정이 높은 비용(높은 시스템 자원소비, 시간증가) 이 드는 경우  <br>
-추가 기능을 넣어야하는데 원본 객체를 수정 하기 힘든 경우  <br>
 <br>

주의사항 : 프록시는 흐름제어만 하고 결과값을 조작하거나 변경 시키면 안된다. <br>