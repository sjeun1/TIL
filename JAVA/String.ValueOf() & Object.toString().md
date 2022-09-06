## String.ValueOf() & Object.toString()
두가지 모두 String 타입으로 변환 시키는 것은 동일하지만 차이점이 있다면 <br>
String 클래스의 valueOf() 메서드는 정적이고 toString() 메서드는 정적이 아니다.<br>
그리고 valueOf는 "null" 이라는 문자열로 처리하지만 <br>
인수가 null 인 경우에 toString은 Null PointerException(NPE)을 발생시키므로<br>
toString 보다 valueOf를 사용하는 것이 더 좋다.<br>
<br>
![image](https://user-images.githubusercontent.com/62210870/188650542-f4cd7987-75db-4b17-9aad-88ea74b6947c.png)
