

### Executor

#### ExecutorService
쓰레드풀을 생성하여 병렬처리. <br />
 ExecutorService에 Task만 지정해주면 알아서 ThreadPool을 이용해 Task를 실행하고 관리한다.<br />
ForkJoinPool가 조금 다른 부분은 Task의 크기에 따라 분할하고, 분할된 Task가 처리되면 그것을 합쳐 리턴한다는 점에서 차이가 있다. <br />
[ForkJoinPool](https://github.com/sjeun1/TIL/blob/main/JAVA/ForkJoinPool.md)
   
