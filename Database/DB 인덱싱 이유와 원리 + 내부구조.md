
# DB 인덱싱 이유와 원리 + 내부구조
<br>
장점 : 테이블 검색 속도와 성능 향상 -> 시스템 부하 줄임<br>
단점 : 인덱스 관리를 위한 추가작업, 추가 저장 공간 필요<br>
<br>
## 주의
<br>
인덱스를 너무 많이 추가하는 경우 더 안좋음 (금방 찾을 걸 인덱스를 사용해 찾느라 느려짐)<br>
컬럼값에 null 이 많이 들어가는 경우 <br>
삽입, 수정, 삭제 작업이 자주 발생하지 않는 컬럼에 사용<br>
where 나 order by, join 등이 자주 사용되는 컬럼에 사용<br><br>
데이터의 중복도가 낮은 컬럼에 사용<br>
<br>

인덱스 조회
SHOW INDEX FROM 테이블명;<br>
<br>
인덱스 생성
CREATE INDEX 인덱스명 ON 테이블명 (컬럼명);<br>
ALTER TABLE 테이블명 ADD INDEX 인덱스명 (컬럼명);<br>
<br>
인덱스 삭제
ALTER TABLE 테이블명 DROP INDEX 인덱스명;<br>
<br>
인덱스 확인
EXPLAIN SELECT * FROM 테이블명; // TYPE = NULL, Possible keys = NULL 이면 인덱스 안타는 경우<br>
<br>

## 내부구조
B-Tree or B+Tree <br>
<br>
자료구조는 선형, 비선형으로 나뉘는데 비선형 종류는 2진트리 -> B-Tree <br>
주소록 같은 거 만들 때 고성능을 내려면 B-Tree 로 구성을 해야함<br>
데이터 10만건일 때에도 속도가 일정하게 유지되도록 해야함<br>
<br>
system, s/w 중에 kernel 이라는 말을 쓰는게 OS, DB 총 2가지가 있음<br>
인덱스 사용 시 빠른 검색도 중요하지만 내가 무엇을 모르는가 판단하는 거.<br>
6이 있는지 검색할 때 없는 데이터 <br>
<br>
선형검색은 6번을 다 찾아야 6이 없는 걸 알수 있다. 5-4-1-3-7-9<br>
비선형 구조를 이용하면 <br>
&nbsp;&nbsp;&nbsp;5<br>
&nbsp;&nbsp;4&nbsp;&nbsp;7<br>
&nbsp;1&nbsp;&nbsp;&nbsp;&nbsp;9<br>
&nbsp;&nbsp;3<br>
일 때 2번의 조회만으로 6 여부를 알 수 있다.<br>
<br>
<br>
<br>
<br>
