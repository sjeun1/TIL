
## mysql
mysql
기본구조<br>
Parser <br>
쿼리를 토큰으로 나눠 트리 형태의 구조로 만듬<br>
기본 문법 오류 체크 <br>

Preprocessor <br>
Parse tree 로 sql 문장구조 체크<br>

Query optimizer <br>
Sql 에 대한 실행 계획을 수립<br>
수립 방법은 role based optimizer, case based optimizer <br>

Query execution engine<br>
만들어진 실행 계획대로 각 핸들러에게 요청<br>



### Replication (복제)
Mysql reolication 이란, 복제를 뜻하며 2대 이상의 dbms 를 나눠서 데이터를 저장하는 방식.<br>
데이터 서버의 부하분산을 하거나 데이터를 실시간으로 백업할 때 사용.<br>
일반적 서비스 구성<br>
Client - web - db<br>

다수의 web 서버를 통한 부하분산<br>
Client - web123 - db<br>

Db 확장<br>
Master - slave<br>

Db read/write 분리, 분산<br>
Client - web - write db or read db <br>

-데이터/스키마 변경과 select connection 분리
데이터/스키마 변경 : Master ip <br>
Select : Slave ip<br>

-업무시 slave 에서는 데이터 변경 하면 안됨<br>
-데이터변경 후 바로 변경된 그 데이터를 조회해야하는 경우 select 는 master 로 한다. <br>
Mysql replication 은 기본적으로 async 하다.<br>
Connection pool size 조정 전 dab 와 협의 <br>
max_user_connection 수치보다 더 많은 connection 사용 불가.<br>

계정 권한 정책<br>
서비스 계정과 ddl 계정의 분리 <br>

<br><br><br>

문법 실행 순서 <br>
FROM - WHERE - GROUP BY - HAVING - SELECT - DISTINCT - ORDER BY <br>
첫번째로 FROM 절로 테이블의 모든 데이터를 가져온 후 WHERE 조건에 일치하는 데이터만 가져온다. <br>
GROUP BY 로 그룹화하여 단일 값으로 축소되고 HAVING 의 조건을 가져온다. <br>
SELECT 의 열을 가져오고 DISTINCT 로 중복을 제거한 후, ORDER bY 로 순서를 정렬한다. <br>

