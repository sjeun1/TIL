## Partitioning(파티셔닝)
<br>

### Partitioning 이란?
파티션을 작은 단위로 나눠서 관리하는 것이다.<br>
보통 회사에서 관리하는 데이터는 많은 양을 가지고 있고,  많은 데이터가 존재하는 테이블이 존재하는데 그만큼 장애가 생길 가능성도 늘어난다고 한다. <br>
데이터가 많으면 데이터베이스의 용량의 한계와 성능의 저하라는 이슈가 생기는데 이를 해결하기 위해 파티셔닝 기법을 이용한다. <br>
<br>

### Partitioning 특징
Query, DML 성능 및 가용성 향상, 관리 용이<br>
필요한 부분만 빠르게 조회해서 성능이 향상 <br>
join 에 대한 비용 증가<br>
table 과 index 를 별도로 파티셔닝 불가능<br>
<br>

### Partitioning 사용
일반적으로 Spring Batch에 대표적으로 사용되는 Scalling 기능 중 하나이다. <br>
