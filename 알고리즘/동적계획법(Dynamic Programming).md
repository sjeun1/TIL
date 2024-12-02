## 동적계획법(Dynamic Programming)

동적 프로그래밍(Dynamic Programming, DP)은 큰 문제를 작은 문제로 나누어 푸는 전략이다. <br>
이미 계산한 결과를 저장해두고 재사용함으로써 계산량을 줄이는 것이 핵심 개념으로, <br>
DP의 대표적인 두 가지 방식은 탑다운(Top-Down)과 바텀업(Bottom-Up) 이 있다. <br>

탑다운(Top-Down) 방식 : 큰 문제를 해결하기 위해 작은 문제를 재귀적으로 호출하며, 결과를 저장해둔다. <br>
바텀업(Bottom-Up) 방식 : 작은 문제를 먼저 해결하고, 이를 이용해 큰 문제를 순차적으로 해결한다. <br>

<br>

### 문제
```
문제 설명:
피보나치 수열은 아래와 같이 정의됩니다:

F(0) = 0, F(1) = 1 
F(n) = F(n-1) + F(n-2) (n ≥ 2)

n번째 피보나치 수를 구하세요.
```

### 답변
해결1. 탑다운(Top-Down) <br>
시간 복잡도 / 공간복잡도 : 𝑂(𝑛) <br>
```java
import java.util.HashMap;
import java.util.Map;

public class FibonacciTopDown {
    private static Map<Integer, Long> memo = new HashMap<>();

    public static long fibonacci(int testNumber) {
        if (testNumber == 0) return 0;
        if (testNumber == 1) return 1;

        if (memo.containsKey(testNumber)) {
            return memo.get(testNumber);
        }

        long result = fibonacci(testNumber - 1) + fibonacci(testNumber - 2);
        memo.put(testNumber, result);

        return result;
    }

    public static void main(String[] args) {
        int testNumber = 50;
        System.out.println("Fibonacci(" + testNumber + ") = " + fibonacci(testNumber));
    }
}

```
<br>

해결2. 바텀업(Bottom-Up)_배열저장 <br>
시간 복잡도 / 공간복잡도 : 𝑂(𝑛) <br>
```java
public class FibonacciBottomUp {
    public static long fibonacci(int testNumber) {
        if (testNumber == 0) return 0;
        if (testNumber == 1) return 1;

        long[] dp = new long[testNumber + 1];
        dp[0] = 0;
        dp[1] = 1;

        for (int i = 2; i <= testNumber; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }

        return dp[testNumber];
    }

    public static void main(String[] args) {
        int testNumber = 50;
        System.out.println("Fibonacci(" + testNumber + ") = " + fibonacci(testNumber));
    }
}

```
<br>

해결3. 바텀업(Bottom-Up)_변수저장 <br>
시간복잡도 : 𝑂(1) <br>
공간복잡도 : 𝑂(𝑛) <br>
```java
public class FibonacciOptimized {
    public static long fibonacci(int testNumber) {
        if (testNumber == 0) return 0;
        if (testNumber == 1) return 1;

        long prev1 = 0, prev2 = 1; 
        long current = 0;

        for (int i = 2; i <= testNumber; i++) {
            current = prev1 + prev2;
            prev1 = prev2;
            prev2 = current;
        }

        return current;
    }

    public static void main(String[] args) {
        int testNumber = 50; 
        System.out.println("Fibonacci(" + testNumber + ") = " + fibonacci(testNumber));
    }
}

```
<br>

2, 3 둘 다 바텀업 저장이지만, 결과값만 필요하거나 메모리 사용이 제한적일 때는 변수 저장이 유리하고, <br>
중간값을 재사용할 때나 최적 경로 문제, 부분합 문제, 문제열 비교 문데 등 다차원 DP 테이블이 필요한 경우, <br>
디버깅 및 검증이 필요할 때 배열 저장이 효율적이다. <br>













