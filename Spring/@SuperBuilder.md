
## @SuperBuilder



상속관계에 있는 entity 에 @Builder 적용 시 오류

#### error  <br>
java: builder() in '{}' cannot override builder() in '' return type '{}'  is not compatible with '{}'

#### result  <br>
@Builder 어노테이션을 사용한 초기화 시에 자식 클래스에서 부모 클래스의 멤버 변수를 발견하지 못하고 <br>
자식 클래스를 통해서 부모 멤버 변수를 초기화하지 못하기 때문에 발생하는 문제이다. <br>
@SuperBuilder 어노테이션은 부모 객체를 상속받는 자식 객체를 만들 때, 부모 객체의 필드값도 지정할 수 있게 해준다. <br>
 <br>

#### code

```
// before

  @Getter
  @Setter
  @Builder
  @NoArgsConstructor
  @AllArgsConstructor
  @FieldDefaults(level = AccessLevel.PRIVATE)
  @ApiModel("request.data")
  public static class data extends base {

    ...
  
  }
```
 <br>


```
// after

  @Getter
  @Setter
  @SuperBuilder
  @NoArgsConstructor
  @AllArgsConstructor
  @FieldDefaults(level = AccessLevel.PRIVATE)
  @ApiModel("request.data")
  public static class data extends base {

    ...
  
  }
```

  
