# [문제링크](https://www.acmicpc.net/problem/1789)

## 📝 문제

서로 다른 N개의 자연수의 합이 S라고 한다. S를 알 때, 자연수 N의 최댓값은 얼마일까?

## 입력

첫째 줄에 자연수 S(1 ≤ S ≤ 4,294,967,295)가 주어진다.

## 출력

첫째 줄에 자연수 N의 최댓값을 출력한다.

## 예제 입력 1 

200

## 예제 출력 1 

19

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        long N = Long.parseLong(br.readLine());

        long sum = 0;
        long number = 0;
        while (sum < N) {
            number++;
            sum += number;
        }

        System.out.println(sum == N ? number : number - 1);
    }
}
```
- N이 최대가 나오려면 숫자를 최대한 작은 수부터 더해가야 한다.
- 만약 N = 12 라면, 1부터 더해서 12와 같거나 초과하는 순간을 봐야하는데
	- 1 + 2 + 3 + 4 + 5 = 15
- 5가 나오는 순간 12를 넘기 때문에 5는 올 수가 없고 4를 늘려서 12를 맞추면 되는 것이다.
- 만약 N = 15라면
	- 1 + 2 + 3 + 4 + 5 = 15
- 작은 숫자를 차례로 더해서 나오는 개수가 최댓값이 된다.
- 따라서, 기본 로직은 1부터 계속 더해주되, 누적값이 N과 같다면 마지막에 더한 값을 리턴해주면 되고 누적값을 초과했다면 마지막에 더한 값은 올 수가 없고 그 이전 값을 수정해서 맞춰야 하므로 이전 값을 리턴해주면 된다!