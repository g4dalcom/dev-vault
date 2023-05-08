# [문제링크](https://www.acmicpc.net/problem/2512)

## 📝 문제

국가의 역할 중 하나는 여러 지방의 예산요청을 심사하여 국가의 예산을 분배하는 것이다. 국가예산의 총액은 미리 정해져 있어서 모든 예산요청을 배정해 주기는 어려울 수도 있다. 그래서 정해진 총액 이하에서 **가능한 한 최대의** 총 예산을 다음과 같은 방법으로 배정한다.

1.  모든 요청이 배정될 수 있는 경우에는 요청한 금액을 그대로 배정한다.
2.  모든 요청이 배정될 수 없는 경우에는 특정한 **정수** 상한액을 계산하여 그 이상인 예산요청에는 모두 상한액을 배정한다. 상한액 이하의 예산요청에 대해서는 요청한 금액을 그대로 배정한다. 

예를 들어, 전체 국가예산이 485이고 4개 지방의 예산요청이 각각 120, 110, 140, 150이라고 하자. 이 경우, 상한액을 127로 잡으면, 위의 요청들에 대해서 각각 120, 110, 127, 127을 배정하고 그 합이 484로 가능한 최대가 된다. 

여러 지방의 예산요청과 국가예산의 총액이 주어졌을 때, 위의 조건을 모두 만족하도록 예산을 배정하는 프로그램을 작성하시오.

## 입력

첫째 줄에는 지방의 수를 의미하는 정수 N이 주어진다. N은 3 이상 10,000 이하이다. 다음 줄에는 각 지방의 예산요청을 표현하는 N개의 정수가 빈칸을 사이에 두고 주어진다. 이 값들은 모두 1 이상 100,000 이하이다. 그 다음 줄에는 총 예산을 나타내는 정수 M이 주어진다. M은 N 이상 1,000,000,000 이하이다. 

## 출력

첫째 줄에는 배정된 예산들 중 최댓값인 정수를 출력한다. 

## 예제 입력 1 

4
120 110 140 150
485

## 예제 출력 1 

127

## 예제 입력 2 

5
70 80 30 40 100
450

## 예제 출력 2

100

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {
    static int[] budget;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());

        StringTokenizer st = new StringTokenizer(br.readLine());

        budget = new int[N];
        for (int i = 0; i < N; i++) {
            budget[i] = Integer.parseInt(st.nextToken());
        }
        Arrays.sort(budget);

        int M = Integer.parseInt(br.readLine());

        int lo = 0;
        int hi = budget[budget.length-1];

        while (lo <= hi) {
            int mid = (lo + hi) / 2;
            int check = sum(mid);

            /**
             * while 종료 조건 외 다른 종료 조건이 없기 때문에
             * hi 값은 최대한 check > M 인 상태까지 간 후 mid - 1이 되기 때문에
             * 결국 check가 M을 넘어가기 직전값까지 가게 되므로 hi를 출력하는 것!
             */
            if (check > M) {
                hi = mid - 1;
            } else {
                lo = mid + 1;
            }
        }

        System.out.println(hi);
    }

    public static int sum(int amount) {
        int result = 0;

        for (int i = 0; i < budget.length; i++) {
            if (budget[i] >= amount) {
                result += amount;
            } else {
                result += budget[i];
            }
        }

        return result;
    }
}
```