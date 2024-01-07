
## annotation Validation check

api 를 개발할 때 request parameter 를 확인하기 위해서 쓰는 방법은 총 3가지가 있다.  <br>
1.spring 에서 제공하는 null check utils 을 사용하는 방법 <br>
2.조건문을 사용하여 null 체크를 하는 방법 <br>
3. api의 request parameter 를 체크하기 위해 사용하는 annotation 을 사용해서 체크 하는 방법<br>

여기서는 3번 어노테이션을 통한 null 체크 방법 중 많이 쓰이는 어노테이션 들의 차이점을 정리하였다. <br> 

### NotNull, NotEmpty, NotBlank 의 차이점
@NotNull<br>
허용X : Null <br>
허용O : "" , " "  <br>
 
@NotEmpty<br>
허용X : Null, ""<br>
허용O : " "  <br>
 
@NotBlank<br>
허용X : Null, "" , " " <br>
 
