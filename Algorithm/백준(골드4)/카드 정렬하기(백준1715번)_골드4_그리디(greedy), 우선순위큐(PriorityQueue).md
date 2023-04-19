# [문제링크](https://www.acmicpc.net/problem/1715)

## 📝 문제

정렬된 두 묶음의 숫자 카드가 있다고 하자. 각 묶음의 카드의 수를 A, B라 하면 보통 두 묶음을 합쳐서 하나로 만드는 데에는 A+B 번의 비교를 해야 한다. 이를테면, 20장의 숫자 카드 묶음과 30장의 숫자 카드 묶음을 합치려면 50번의 비교가 필요하다.

매우 많은 숫자 카드 묶음이 책상 위에 놓여 있다. 이들을 두 묶음씩 골라 서로 합쳐나간다면, 고르는 순서에 따라서 비교 횟수가 매우 달라진다. 예를 들어 10장, 20장, 40장의 묶음이 있다면 10장과 20장을 합친 뒤, 합친 30장 묶음과 40장을 합친다면 (10 + 20) + (30 + 40) = 100번의 비교가 필요하다. 그러나 10장과 40장을 합친 뒤, 합친 50장 묶음과 20장을 합친다면 (10 + 40) + (50 + 20) = 120 번의 비교가 필요하므로 덜 효율적인 방법이다.

N개의 숫자 카드 묶음의 각각의 크기가 주어질 때, 최소한 몇 번의 비교가 필요한지를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N이 주어진다. (1 ≤ N ≤ 100,000) 이어서 N개의 줄에 걸쳐 숫자 카드 묶음의 각각의 크기가 주어진다. 숫자 카드 묶음의 크기는 1,000보다 작거나 같은 양의 정수이다.

## 출력

첫째 줄에 최소 비교 횟수를 출력한다.

## 예제 입력 1 

3
10
20
40

## 예제 출력 1

100

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.PriorityQueue;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());

        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int i = 0; i < N; i++) {
            pq.offer(Integer.parseInt(br.readLine()));
        }

        int sum = 0;
        while (pq.size() > 1) {
            int calc = pq.poll() + pq.poll();
            sum += calc;
            pq.offer(calc);
        }

        System.out.println(sum);
    }
}
```
- 우선 예제 10, 20, 40으로 규칙을 찾아보면,
- (10 + 20) + (10 + 20 + 40) 이런 식으로 **누적값 + 이전 결과값 + 다음 수** 가 된다.
- 어차피 정렬이 되어있으니 순차적으로 더하면 되지 않을까라는 안일한 생각을 하기 쉬운 것 같다.
- 처음에는 dp를 이용해 이전 누적값을 계속 넘겨받는 식으로 풀어보려고 했는데,
- 1, 2, 3, 4, 5 를 같은 규칙을 적용해보면
- (1 + 2) + (3 + 3) + (6 + 4) + (10 + 5) = 34 이다.
- 그러나 실제로는 (1 + 2) + (3 + 3) + (4 + 5) + (6 + 9) = 33으로 해야 최솟값이 나온다.
- 이전 결과값보다 두 개의 더 작은 수들이 존재한다면 그 수들을 먼저 계산해야 하는 것이다.
- 그래서 calc 라는 값에 이전 결과값을 담아서 우선순위큐에 담아주고 calc를 포함하여 남은 수들 중 가장 작은 두 개의 숫자를 계속해서 꺼내며 누적값에 더하는 방식으로 구하였다!