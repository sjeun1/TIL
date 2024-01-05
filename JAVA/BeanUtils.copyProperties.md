
## BeanUtils.copyProperties
스프링에서 기본으로 제공해주는 메소드로 원본객체를 복사할 때 사용 <br>

#### 주의
1.얕은 복사로 복사가 됨  <br>
2. 사용했을 때 IllegalAccessException e, InvocationTargetException e 예외처리 번거로움이 있었음 <br>
3. Reflection(** 리플렉션은 프로그램이 실행 중에 자신의 구조를 살펴보고 수정할 수 있는 능력을 의미함) 을 사용하여 객체의 값을 복사하는데, <br>
객체의 값을 복사하기 위해서는 복사할 원본 객체(Source)의 멤버 변수에 대한 get 메서드가 있어야 하고  <br>
값이 복사될 대상 객체(Target)의 멤버 변수에 대한 set 메서드가 있어야 했다.  <br>

BeanUtils.copyProperties 대신에 다른 걸 찾아보았다. <br><br>



#### 대체
총 3가지 유사한 라이브러리를 찾았는데 modelMapper 사용을 하였다. (modelMapper 도 객체 간에 속성 값을 복사하는 데 사용) <br><br>

1.modelMapper
```
import org.modelmapper.ModelMapper;

// ModelMapper 인스턴스 생성
ModelMapper modelMapper = new ModelMapper();

// 속성 복사
modelMapper.map(sourceObject, targetObject);
```



2.MapStruct
```
import org.mapstruct.Mapper;
import org.mapstruct.factory.Mappers;

// Mapper 인터페이스 정의
@Mapper
public interface MyMapper {
    MyMapper INSTANCE = Mappers.getMapper(MyMapper.class);

    // 다양한 매핑 메서드 정의
    TargetObject map(SourceObject source);
}

// 속성 복사
TargetObject targetObject = MyMapper.INSTANCE.map(sourceObject);
```




3.Orika 
```
import ma.glasnost.orika.MapperFactory;
import ma.glasnost.orika.impl.DefaultMapperFactory;

// MapperFactory 인스턴스 생성
MapperFactory mapperFactory = new DefaultMapperFactory.Builder().build();

// 속성 복사
mapperFactory.getMapperFacade().map(sourceObject, targetObject);
```
