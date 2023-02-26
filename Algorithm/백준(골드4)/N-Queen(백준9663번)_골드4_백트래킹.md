# [문제링크](https://www.acmicpc.net/problem/9663)

## 📝 문제

N-Queen 문제는 크기가 N × N인 체스판 위에 퀸 N개를 서로 공격할 수 없게 놓는 문제이다.

N이 주어졌을 때, 퀸을 놓는 방법의 수를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N이 주어진다. (1 ≤ N < 15)

## 출력

첫째 줄에 퀸 N개를 서로 공격할 수 없게 놓는 경우의 수를 출력한다.

## 예제 입력 1 

8

## 예제 출력 1 

92

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    static int N;
    static int[] board;
    static int count = 0;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());

        // 인덱스는 열, 값은 행!
        board = new int[N];

        dfs(0);
        System.out.println(count);
    }

    public static void dfs(int depth) {
        if (depth == N) {
            count++;
            return;
        }

        for (int i = 0; i < N; i++) {
            board[depth] = i;
            if (possible(depth)) {
                dfs(depth + 1);
            }
        }
    }

    public static boolean possible(int x) {

        /**
         * 행이 같은 경우와 (board[x] == board[i])
         * 대각선 위치인 경우 (각 행의 차이와 열의 차이가 같을 때) false를 반환한다.
         */
        for (int i = 0; i < x; i++) {
            if (board[x] == board[i]) {
                return false;
            } else if (Math.abs(x - i) == Math.abs(board[x] - board[i])) {
                return false;
            }
        }
        return true;
    }
}
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbCzEsa%2Fbtr0Oaur1Kq%2FM6Kb9ZDlTsk9BKOi6LcBVk%2Fimg.png)
- 백트래킹 방식을 기본으로 풀이를 하였다.
- 왼쪽 그림처럼 board[2]에서 퀸을 놓을 자리가 없기 때문에 depth 0으로 다시 돌아가고(재귀)
- 오른쪽 그림처럼 모두 퀸을 배치한 경우 카운팅이 된다.