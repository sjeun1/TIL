## System.lineSeparator() ( =System.getProperty("line.separator"))
개행문자란 줄바꿈을 나타내는 제어 문자이다. <br>
개행문자에는 LF(\n) 과 CR(\r) 이 있다.<br>
이러한 개행문자는 운영체제 별로 차이가 있어서 System.getProperty("line.separator") 를 쓴다.<br>

#### 운영체제 별 개행문자 <br>
Window : CRLF (\r\n)<br>
Mac : CR(\r)<br>
Unix : LF(\n)<br>

#### 버전별 사용 방법
Java 버전 6 이하  :  System.getProperty("line.separator")
Java 버전 7 이상  :  System.lineSeparator()
<br>

#### 문제
String text = "개행문자가\n 포함되어있습니다.\n";<br>
해당 텍스트로 System.lineSeparator() 를 조회를 했을 때 한글이므로 KR의 값을 기대했는데 EN으로 값이 조회되어 원하는 값이 나오지 않고 있었다.<br>

#### 원인
Windows platform 에서는 "\r\n" (CR+LF) 으로 표시되었기 때문에 개행문자가 공백으로 replace 되지않았다. 
참고 사이트 : https://stackoverflow.com/questions/3516957/how-do-i-use-system-getpropertyline-separator-tostring


#### 해결방안

![image](https://user-images.githubusercontent.com/62210870/192455586-09d6136e-e757-4cd5-817b-443f2c59e138.png)

