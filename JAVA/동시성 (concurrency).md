
## 동시성 (concurrency)

동시성이란, 동시에 실행중인 것처럼 보이는 프로그래밍 기법을 말한다. <br>
동시성은 작업들이 동시에 실행되는 것처럼 보이도록 관리하고, 작업들 간의 실행 순서를 관리하며 자원을 효율적으로 할당한다. <br>

병렬성과 헷갈릴 수 있는데 병렬성은 실제로 여러 작업이 동시에 병렬적으로 실행되는 것으로 멀티코어 프로세서와 같은 병렬 처리 장치에서 가능하며 동시성과 구분이 필요하다. <br>
<br>


### 동시성 처리 방법
#### 1. 스레드 (Thread)
   - 자바에서는 다중 스레드를 통해 동시성을 구현할 수 있으며 각 스레드는 독립적으로 실행된다. <br>
```
public class Runnable implements Runnable {
    
    @Override
    public void run() {
        System.out.println("스레드가 실행할 작업");
    }
    
    public static void main(String[] args) {
        Runnable runnable = new MyRunnable();
        
        Thread thread = new Thread(runnable);
        thread.start();
        
        System.out.println("메인 스레드에서도 다른 작업을 수행 가능");
    }
}
```
<br><br>

#### 2. 동기화 (Synchronization):
   - 여러 스레드가 공유 자원(메모리, 파일 등)에 접근할 때 발생할 수 있는 데이터 불일치 문제를 해결하기 위한 기법으로, synchronized 키워드나 java.util.concurrent 패키지의 동기화 메커니즘을 사용한다. <br>

  2-1. 메서드 전체에 적용
```
public class SynchronizationExample {
    private static int counter = 0;

    public static void main(String[] args) {
        Thread thread1 = new Thread(new CounterIncrementer());
        Thread thread2 = new Thread(new CounterIncrementer());

        thread1.start();
        thread2.start();

        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        // 최종 결과 출력
        System.out.println("최종값: " + counter);
    }

    static class CounterIncrementer implements Runnable {
        @Override
        public void run() {
            for (int i = 0; i < 1000; i++) {
                incrementCounter();
            }
        }

        // synchronized 키워드를 사용하여 동기화된 메서드
        private synchronized void incrementCounter() {
            counter++;
        }
    }
}
```

  2-2. 특정코드 블록에 적용
```
public class SynchronizationBlockExample {

    private static int counter = 0;
    private static final Object lock = new Object();

    public static void main(String[] args) {
        Thread thread1 = new Thread(new CounterIncrementer());
        Thread thread2 = new Thread(new CounterIncrementer());

        thread1.start();
        thread2.start();

        try {
            thread1.join();
            thread2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println("최종값: " + counter);
    }

    static class CounterIncrementer implements Runnable {
        @Override
        public void run() {
            for (int i = 0; i < 1000; i++) {
                incrementCounter();
            }
        }

        private void incrementCounter() {
            // synchronized 블록을 사용하여 블록 단위로 동기화
            synchronized (lock) {
                counter++;
            }
        }
    }
}
```  
<br><br>

#### 3. 락 (Lock)
   - 암시적 Lock (synchronized)
    문제가 된 메서드, 변수에 각각 키워드 추가한다. 이 키워드를 사용하면 객체에 포함된 다른 모든 synchronized 의 접근까지 lock 이 걸린다. <br>
    자바 내부적으로 block 과 unblock 을 처리하게 되는데 이런 처리들이 너무 많아 사용 되면 오히려 프로그램 성능 저하를 일으킬 수 있다. <br>
   -  명시적 Lock
     해당 Lock 의 범위를 메서드 내부에서 한정하기 어렵거나 동시에 여러 Lock 을 사용하고 싶을 때 사용한다. <br>
     ReentrantLock, ReentrantReadWriteLock 등이 있다. <br>
<br><br>

#### 4. 스레드 풀 (Thread Pool) 
   - 자주 사용되는 스레드를 미리 생성하여 관리하고, 작업을 큐에 넣어 스레드풀에서 순차적으로 처리하게 하는 방식이다. <br>
    필요한만큼 스레드를 효율적으로 재사용할 수 있고, 스레드 생성과 종료에 따른 오버헤드를 줄이고 성능을 향상 시킬 수 있는 특징이 있다. <br>
```
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadPoolExample {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(5);

        for (int i = 0; i < 10; i++) {
            executorService.submit(new MyTask(i));
        }

        executorService.shutdown(); // 작업 완료될 때까지 대기
    }

    static class MyTask implements Runnable {
        private final int taskId;

        public MyTask(int taskId) {
            this.taskId = taskId;
        }

        @Override
        public void run() {
            // 작업 내용 작성
            System.out.println("Task ID : " + taskId + " - 스레드 이름 : " + Thread.currentThread().getName());
        }
    }
}
```

동시성을 잘못 사용하게 되면 데드락, 경쟁 조건 등의 문제가 발생할 수 있다. <br>
<br>

* 데드락 : 2개 이상의 작업들이 서로 기다리며 무한히 대기하게 되는 상황이다. 상호배제, 점유대기, 비선점, 순환대기 총 4가지 조건이 동시에 만족할 때 발생한다. <br>
* 경쟁조건 : 2개 이상의 스레드나 프로세스가 공유 자원에 동시에 접근하려고 할 때 발생할 수 있는 상황이다. 일반적으로 여러 스레드 접근-> 그작업 들이 서로의 실행 순서에 따라 결과가 달라질 수 있을 때 일반적으로 발생한다. <br>


