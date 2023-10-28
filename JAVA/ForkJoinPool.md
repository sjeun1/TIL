

### ForkJoinPool

java7에서 지원. fork-join pool 은 병렬 작업을 효율적으로 관리하고 실행 할 수 있는 자바 라이브러리이다. <br/>
큰 업무를 작은 업무로 배분하고, 이후 다시 취합하는 방식으로 동작한다. <br />
큰 문제를 작은 부분 문제로 분할하고, 해결한 후 그 결과를 결합하여 해결을 하는 알고리즘인 분할 정복 알고리즘과 비슷하다.<br />
submit 하면 하나의 큐에 누적되고, 그걸 쓰레드가 자신의 개별 큐에서 처리한다. <br/>
<br/>

```
// thread 10개 사용하며, 각 thread 당 100개씩 할당하여 datalist 쌓는 예제

List<List<VO>> partitionList = ListUtils.partition(List, 100);
List<VO> dataList = Collections.synchronizedList(new ArrayList<>());

ForkJoinPool forkJoinPool = new ForkJoinPool(10);
forkJoinPool.submit(() -> {
  partitionList.parallelStream().forEach(list -> {
    try {
      // logic
      dataList.addAll(list);
    } catch (Exception e) {
      log.error(e.getMessage());
    }
  });
}).get();
```
<br/>
<br/>
참고 : https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ForkJoinPool.html
