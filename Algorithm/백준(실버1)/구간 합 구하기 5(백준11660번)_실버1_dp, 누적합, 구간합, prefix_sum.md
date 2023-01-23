# [문제링크](https://www.acmicpc.net/problem/11660)

## 📝 문제

N×N개의 수가 N×N 크기의 표에 채워져 있다. (x1, y1)부터 (x2, y2)까지 합을 구하는 프로그램을 작성하시오. (x, y)는 x행 y열을 의미한다.

예를 들어, N = 4이고, 표가 아래와 같이 채워져 있는 경우를 살펴보자.

|    |    |    |   |
|:--- |:--- |:--- |:--- |
| 1   | 2   | 3   | 4   |
| 2   | 3   | 4   | 5   |
| 3   | 4   | 5   | 6   |
| 4   | 5   | 6   | 7   |


여기서 (2, 2)부터 (3, 4)까지 합을 구하면 3+4+5+4+5+6 = 27이고, (4, 4)부터 (4, 4)까지 합을 구하면 7이다.

표에 채워져 있는 수와 합을 구하는 연산이 주어졌을 때, 이를 처리하는 프로그램을 작성하시오.

## 입력

첫째 줄에 표의 크기 N과 합을 구해야 하는 횟수 M이 주어진다. (1 ≤ N ≤ 1024, 1 ≤ M ≤ 100,000) 둘째 줄부터 N개의 줄에는 표에 채워져 있는 수가 1행부터 차례대로 주어진다. 다음 M개의 줄에는 네 개의 정수 x1, y1, x2, y2 가 주어지며, (x1, y1)부터 (x2, y2)의 합을 구해 출력해야 한다. 표에 채워져 있는 수는 1,000보다 작거나 같은 자연수이다. (x1 ≤ x2, y1 ≤ y2)

## 출력

총 M줄에 걸쳐 (x1, y1)부터 (x2, y2)까지 합을 구해 출력한다.

## 예제 입력 1 복사

4 3
1 2 3 4
2 3 4 5
3 4 5 6
4 5 6 7
2 2 3 4
3 4 3 4
1 1 4 4

## 예제 출력 1 복사

27
6
64

---

### ❌ 처음 풀이(시간 초과)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int[][] board;
    static StringBuilder sb = new StringBuilder();

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        board = new int[N+1][N+1];

        for (int i = 1; i <= N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 1; j <= N; j++) {
                board[j][i] = Integer.parseInt(st.nextToken());
            }
        }

        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());

            int x1 = Integer.parseInt(st.nextToken());
            int y1 = Integer.parseInt(st.nextToken());
            int x2 = Integer.parseInt(st.nextToken());
            int y2 = Integer.parseInt(st.nextToken());

            calculate(x1, y1, x2, y2);
        }
        System.out.println(sb);
    }

    public static void calculate(int x1, int y1, int x2, int y2) {
        int sum = 0;

        for (int i = y1; i <= y2; i++) {
            for (int j = x1; j <= x2; j++) {
                sum += board[i][j];
            }
        }
        sb.append(sum).append("\n");
    }
}
```
- 입력값을 그대로 받아서 반복문으로 계산을 하게 되면 시간초과가 발생한다.... 이대로 계산을 하게 되면 시간복잡도는 O(NM) 이 되기 때문에 시간제한이 걸려있는 경우에는 `prefix sum` 알고리즘을 이용해 O(1) 로 구해야 한다고 한다
- 시간복잡도와 관련된 문제가 가장 까다로운 것 같다.

### 🔎 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        int[][] dp = new int[N+1][N+1];

        /**
         * 누적합 dp 배열에 넣기
         */
        for (int i = 1; i <= N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 1; j <= N; j++) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1] - dp[i-1][j-1] + Integer.parseInt(st.nextToken());
            }
        }

        /**
         * 입력값 받아서 구간합 구하기
         */
        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());

            int x1 = Integer.parseInt(st.nextToken());
            int y1 = Integer.parseInt(st.nextToken());
            int x2 = Integer.parseInt(st.nextToken());
            int y2 = Integer.parseInt(st.nextToken());

            System.out.println(
                    dp[x2][y2] - dp[x1-1][y2] - dp[x2][y1-1] + dp[x1-1][y1-1]
            );
        }
    }
}
```
- [참고 사이트](https://velog.io/@isohyeon/Java-%EB%B0%B1%EC%A4%80-11660-%EA%B5%AC%EA%B0%84-%ED%95%A9-%EA%B5%AC%ED%95%98%EA%B8%B0-5)
- `누적합` 과 `구간합` 의 개념과 알고리즘을 공부해야 했던 문제이다.
- `누적합` 구하는 방법
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F7AS75%2FbtrWU72MbB9%2FVN5A9mmXj2GCDSvF96lMmK%2Fimg.png)

- `구간합` 구하는 방법
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fp8VfR%2FbtrWVKGnjKX%2FO9b8tHkzWBZ1qOSqzjDUyk%2Fimg.png)
- 원리를 대략 이해하고 그림을 기억하면 다음에 적용해서 풀 때 용이할 것 같다.
- solved.ac 에서 문제를 푼 사람 수를 기준으로 정렬하고 순서대로 풀고 있어서 구간 합 구하기 시리즈 중 5를 제일 먼저 풀게 되었는데 구간 합 시리즈를 좀 더 풀어봐야 할 것 같다!