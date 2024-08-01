## jpa 매핑


### 매핑 어노테이션
@Column : 컬럼 <br>
Name : 필드와 매핑할 테이블의 컬럼 이름 <br>
Updatable : 등록, 변경 가능 여부 <br>
Nullable : null 값의 허용 여부를 설정한다. False == not null  <br>
Unique : @Table에 uniqueConstraints 와 같음. 한 컬럼에 간단히 유니크 제약조건 걸때 사용. <br>
columnDefinition : db 컬럼 정보를 직접 줄 때 사용 <br>
Length : 문자 길이 제약조건, string 타입에만 사용 <br>
Precision, scale : bigDecimal, bigInteger 타입에서 사용 <br>

@Enumerated : enum 타입 매핑 <br>
Enum 타입 매핑 할 때 EnumType.ORDINAL (enum 순서 저장) 말고 EnumType.STRING (enum 이름 저장) 을 사용한다. 기본 값은 ORDINAL 이다 <br>

@Temporal : 날짜 타입 매핑 (날짜 그냥 LocalDate, LocalDateTime 을 사용할 때는 date, timestamp 로 저장 되어 생략 가능) <br>
@Lob : blob, clob 매핑 (string, char[] 는 club로 ,  byte[] 는 blob 으로 매핑됨) <br>
@Transient : 메모리에서만 매핑(db 에는 매핑X) <br>
 <br> <br>

### 기본키 매핑
@Id : 직접 할당  <br>
@GeneratedValue : 자동 생성. 옵션으로는 TABLE, SEQUENCE, IDENTITY, UUID, AUTO 가 있다.  <br>
Ex) @GeneratedValue(strategy = GenerationType.IDENTITY) <br>
@SequenceGenerator : oracle 에서 자주 사용. (Int, Integer, Long) <br>
@TableGenerator : 운영에서 잘안씀 <br>
allocationSize 를 사용하면 시퀀스 한번 호출에 증가하는 수를 지정 할 수 있다 (성능 최적화에 사용됨) <br>
 <br> <br>


### 양방향 매핑
둘 중 하나로 외래키를 관리해야한다.  <br>
mappedBy 가 적힌 곳은 다 읽게만 만들어야한다 (조회만 가능) <br>
외래키가 있는 곳을 연관관계의 주인으로 정하기! <br>

단방향 매핑만으로도 이미 연관관계 매핑이 완료되고, 양방향은 반대방향으로 조회가 추가된 것  <br>
양방향 매핑 시 연관관계 양쪽 다 값을 입력해야 함 -> 연관관계 편의 메소드를 생성하면 실수를 줄일 수 있다. <br>
양방향 매핑 시 무한루프에 인해 stackOverFlow 가 생길 수 있어 주의해야한다.  <br>
엔티티를 컨트롤러에서 반환하지말고 dto 를 사용해야한다.  <br>
 <br> <br>
