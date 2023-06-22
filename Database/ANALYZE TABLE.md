## ANALYZE TABLE
테이블, 인덱스, 클러스터의 통계정보를 재생성하여 성능을 최적화하고 키를 재분배하는 것이다. <br>
인덱스를 생성해도 옵티마이저는 실제 쿼리를 실행 할 때 데이터의 형태에 따라 Index Range Scan이 아니라 테이블을 처음부터 끝까지 읽는 방식인 Full Table Scan 으로 실행해 버릴 수 있다. <br>
<br>

테이블을 재생성 하거나, 새로 클러스터링을 한 경우, 인덱스를 추가하거나 재생성한 경우, <br>
다량의 데이터를 SQL이나 배치 애플리케이션을 통해 작업한 경우 ANALYZE를 수행 시켜 주는 것이 좋다.<br>
너무 자주 사용하면 너무 느리기도 하고 테이블 read lock이 걸리기 때문에 조심해서 사용해야함.<br>
<br>

참고<br>
https://dev.mysql.com/doc/refman/8.0/en/analyze-table.html <br>
https://zetawiki.com/wiki/MySQL_ANALYZE_TABLE
