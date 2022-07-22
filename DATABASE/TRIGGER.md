

## TRIGGER(트리거)
<br>

### TRIGGER 란
TRIGGER 는 테이블에 대한 이벤트에 반응해 자동으로 실행되는 작업을 의미한다.
데이터를 CRUD 했을 때 로그를 기록하며 상태 관리를 자동화 하는데 사용이 된다.
<br>

### TRIGGER 의 장점
-&nbsp;업무 처리 자동화 <br>
-&nbsp;데이터 변경한 사용자 추적 <br>
-&nbsp;데이터 무결성의 강화<br>
-&nbsp;데이터 백업<br>

### TRIGGER 의 단점
-&nbsp;성능 저하 <br>
-&nbsp;오류 발생 시 모두 롤백 <br>
-&nbsp;트랜잭션 처리 되어 외부로부터 블로킹 상태 <br>

## 사용이유
-&nbsp;데이터베이스를 임의로 변경 시 경고 메세지 호출할 경우<br>
-&nbsp;주문이 되면 재고 테이블에 수량도 변경되는 등 연관 테이블에 업데이트가 필요한 경우<br>
-&nbsp;수정/삭제한 사용자들의 정보를 알고 싶은 경우 등..<br>
  
## TRIGGER 형식
![image](https://user-images.githubusercontent.com/62210870/180423633-3cc4b963-8f04-4556-b817-f7250056ae8b.png)
