# [문제링크](https://www.acmicpc.net/problem/12865)

## 📝 문제

이 문제는 아주 평범한 배낭에 관한 문제이다.

한 달 후면 국가의 부름을 받게 되는 준서는 여행을 가려고 한다. 세상과의 단절을 슬퍼하며 최대한 즐기기 위한 여행이기 때문에, 가지고 다닐 배낭 또한 최대한 가치 있게 싸려고 한다.

준서가 여행에 필요하다고 생각하는 N개의 물건이 있다. 각 물건은 무게 W와 가치 V를 가지는데, 해당 물건을 배낭에 넣어서 가면 준서가 V만큼 즐길 수 있다. 아직 행군을 해본 적이 없는 준서는 최대 K만큼의 무게만을 넣을 수 있는 배낭만 들고 다닐 수 있다. 준서가 최대한 즐거운 여행을 하기 위해 배낭에 넣을 수 있는 물건들의 가치의 최댓값을 알려주자.

## 입력

첫 줄에 물품의 수 N(1 ≤ N ≤ 100)과 준서가 버틸 수 있는 무게 K(1 ≤ K ≤ 100,000)가 주어진다. 두 번째 줄부터 N개의 줄에 거쳐 각 물건의 무게 W(1 ≤ W ≤ 100,000)와 해당 물건의 가치 V(0 ≤ V ≤ 1,000)가 주어진다.

입력으로 주어지는 모든 수는 정수이다.

## 출력

한 줄에 배낭에 넣을 수 있는 물건들의 가치합의 최댓값을 출력한다.

## 예제 입력 1 

4 7
6 13
4 8
3 6
5 12

## 예제 출력 1

14

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());       // 물건의 개수
        int K = Integer.parseInt(st.nextToken());       // 최대 무게

        int[] weight = new int[N+1];                      // 무게를 담을 배열
        int[] value = new int[N+1];                       // 가치를 담을 배열
        int[][] dp = new int[N+1][K+1];                   // dp[물건번호][무게]

        for (int i = 1; i <= N; i++) {
            st = new StringTokenizer(br.readLine());
            weight[i] = Integer.parseInt(st.nextToken());
            value[i] = Integer.parseInt(st.nextToken());
        }

        int max = Integer.MIN_VALUE;
        for (int i = 1; i <= N; i++) {                      // 아이템 순서대로
            for (int j = 1; j <= K; j++) {                  // 무게(j)에 대해 넣을 수 있는 값을 넣는다.
                dp[i][j] = dp[i-1][j];                      // 기본적으로 이전 아이템의 가치를 저장하되
                if (j - weight[i] >= 0) {                   // 무게에서 자신의 무게를 뺏을 때 여유 무게가 생기면
                    dp[i][j] = Math.max(dp[i-1][j], dp[i-1][j-weight[i]] + value[i]);   // 이전 아이템의 가치 vs 남은 무게의 가치 + 나의 가치 중 큰 값을 저장한다.
                }
                max = Math.max(dp[i][j], max);
            }
        }
        System.out.println(max);
    }
}
```
- dfs로 풀었을 때 예제 출력값은 잘 나왔으나 제출했을 때 계속 틀렸다는 응답을 받았다..
- 검색해보니 `knapsack 알고리즘`이라는 풀이법이 따로 존재하고 DP를 이용해야 한다는 힌트를 얻었는데, 혼자 고민해보다가 dp 배열을 어떻게 설정해야 할지 떠오르지 않아서 다른 풀이를 보고 공부한 뒤에 풀어보았다.
- DP 풀이에서 중요한 것은 DP 배열을 어떻게 설정할 것인지와 점화식을 어떻게 세울 것인지인 것 같다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIusVx%2FbtrXoD0V1Qb%2FCo2gt5oyEq3Z9gM7CXuv01%2Fimg.jpg)
- 1부터 ~ K(최대 무게) 까지 각 무게별로 최대 가치를 저장한다.
- 물건1은 무게가 6이라서 무게 5까지는 담을 수 없으므로 0을 저장하고 무게 6에 13을 저장한다. 무게가 7일 때 물건1의 무게를 빼면 무게 1이 남는데 같은 물건을 담을 수는 없으므로 일단 13을 저장한다.
- 물건2도 똑같이 담아주되 무게 6에서는 물건1을 담는 것이 최댓값이므로 물건1의 값을 그대로 저장하고 무게 7에서도 마찬가지이다. 이 때 무게 5, 6, 7일 때 각각 물건2의 무게를 빼면 1, 2, 3의 무게가 남는데 이전 물건(물건1)의 무게 1, 2, 3일 때 저장된 값은 0이므로 가치의 변화는 없다.
- 물건3일 때도 이전과 같은 방법으로 저장을 한다. 다만 무게 7일 때 물건의3의 무게를 빼면 4의 무게가 남고, 이전 물건(물건2)이 무게 4일 때 8이라는 값을 저장하고 있으므로 비교를 해주어야 한다.
	- 물건2의 값을 그대로 가져오면 = 13
	- 물건3의 가치(6) + 남은 무게로 추가할 수 있는 물건의 가치(8) = 14
	- 이므로 14가 저장된다.
- 물건4는 물건 1, 2와 같은 방식으로 저장한다.


### ❌ 오답 (bfs풀이)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N, K;
    static int[] weight, value;
    static boolean[] visit;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        K = Integer.parseInt(st.nextToken());

        weight = new int[N];
        value = new int[N];

        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());

            weight[i] = a;
            value[i] = b;
        }

        int max = Integer.MIN_VALUE;
        for (int i = 0; i < N; i++) {
            visit = new boolean[N];
            int weightSum = 0;
            int valueSum = 0;
            max = Math.max(max, dfs(i, weightSum, valueSum));
        }
        System.out.println(max);
    }

    public static int dfs(int num, int weightSum, int valueSum) {
        weightSum += weight[num];
        valueSum += value[num];
        visit[num] = true;

        for (int i = num; i < N; i++) {
            if (!visit[i] && K >= weightSum + weight[i]) {
                visit[i] = true;
                dfs(i,weightSum, valueSum);
                weightSum += weight[i];
                valueSum += value[i];
                visit[i] = false;
            }
        }
        return valueSum;
    }
}
```