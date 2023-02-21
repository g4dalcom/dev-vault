# [문제링크](https://www.acmicpc.net/problem/17070)

## 📝 문제

유현이가 새 집으로 이사했다. 새 집의 크기는 N×N의 격자판으로 나타낼 수 있고, 1×1크기의 정사각형 칸으로 나누어져 있다. 각각의 칸은 (r, c)로 나타낼 수 있다. 여기서 r은 행의 번호, c는 열의 번호이고, 행과 열의 번호는 1부터 시작한다. 각각의 칸은 빈 칸이거나 벽이다.

오늘은 집 수리를 위해서 파이프 하나를 옮기려고 한다. 파이프는 아래와 같은 형태이고, 2개의 연속된 칸을 차지하는 크기이다.

![](https://upload.acmicpc.net/3ceac594-87df-487d-9152-c532f7136e1e/-/preview/)

파이프는 회전시킬 수 있으며, 아래와 같이 3가지 방향이 가능하다.

![](https://upload.acmicpc.net/b29efafa-dbae-4522-809c-76d5c184a231/-/preview/)

파이프는 매우 무겁기 때문에, 유현이는 파이프를 밀어서 이동시키려고 한다. 벽에는 새로운 벽지를 발랐기 때문에, 파이프가 벽을 긁으면 안 된다. 즉, 파이프는 항상 빈 칸만 차지해야 한다.

파이프를 밀 수 있는 방향은 총 3가지가 있으며, →, ↘, ↓ 방향이다. 파이프는 밀면서 회전시킬 수 있다. 회전은 45도만 회전시킬 수 있으며, 미는 방향은 오른쪽, 아래, 또는 오른쪽 아래 대각선 방향이어야 한다.

파이프가 가로로 놓여진 경우에 가능한 이동 방법은 총 2가지, 세로로 놓여진 경우에는 2가지, 대각선 방향으로 놓여진 경우에는 3가지가 있다.

아래 그림은 파이프가 놓여진 방향에 따라서 이동할 수 있는 방법을 모두 나타낸 것이고, 꼭 빈 칸이어야 하는 곳은 색으로 표시되어져 있다.

![](https://upload.acmicpc.net/0f445b26-4e5b-4169-8a1a-89c9e115907e/-/preview/)

가로

![](https://upload.acmicpc.net/045d071f-0ea2-4ab5-a8db-61c215e7e7b7/-/preview/)

세로

![](https://upload.acmicpc.net/ace5e982-6a52-4982-b51d-6c33c6b742bf/-/preview/)

대각선

가장 처음에 파이프는 (1, 1)와 (1, 2)를 차지하고 있고, 방향은 가로이다. 파이프의 한쪽 끝을 (N, N)로 이동시키는 방법의 개수를 구해보자.

## 입력

첫째 줄에 집의 크기 N(3 ≤ N ≤ 16)이 주어진다. 둘째 줄부터 N개의 줄에는 집의 상태가 주어진다. 빈 칸은 0, 벽은 1로 주어진다. (1, 1)과 (1, 2)는 항상 빈 칸이다.

## 출력

첫째 줄에 파이프의 한쪽 끝을 (N, N)으로 이동시키는 방법의 수를 출력한다. 이동시킬 수 없는 경우에는 0을 출력한다. 방법의 수는 항상 1,000,000보다 작거나 같다.

## 예제 입력 1 
3
0 0 0
0 0 0
0 0 0

## 예제 출력 1 

1

## 예제 입력 2 

4
0 0 0 0
0 0 0 0
0 0 0 0
0 0 0 0

## 예제 출력 2 

3

## 예제 입력 3 

5
0 0 1 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0
0 0 0 0 0

## 예제 출력 3 

0

## 예제 입력 4 

6
0 0 0 0 0 0
0 1 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0
0 0 0 0 0 0

## 예제 출력 4 

13

---

### 🔎 정답(DP)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N;
    static int[][][] dp;
    static int[][] map;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());

        map = new int[N][N];        // 집의 상태
        for (int i = 0; i < N; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        /**
         * dp는 해당 좌표에 올 수 있는 경우의 수
         * 세 번째 배열은 파이프 방향! 0: 가로, 1: 세로, 2: 대각선
         * j = 0, 1 인 좌표는 도달 불가능하므로 j = 2부터 시작!
         */
        dp = new int[N][N][3];
        dp[0][1][0] = 1;
        for (int i = 0; i < N; i++) {
            for (int j = 2; j < N; j++) {

                /**
                 * 가로 조건
                 * 해당 좌표에 가로인 상태로 놓이려면 이전 좌표에서 가로이거나 대각선으로 놓여있어야 하므로
                 * 해당 dp값을 더해준다!
                 */
                if (map[i][j] == 0) {
                    dp[i][j][0] = dp[i][j-1][0] + dp[i][j-1][2];

                    /**
                     * 세로 조건
                     * 해당 좌표에 세로인 상태로 놓이려면 이전 좌표에서 세로이거나 대각선이어야 함!
                     * 여기서 i가 0인 경우에서는 세로로 놓일 수 없기 때문에 조건을 걸어준다(i-1 >= 0)
                     */
                    if (i-1 >= 0) {
                        dp[i][j][1] = dp[i-1][j][1] + dp[i-1][j][2];

                        /**
                         * 대각선 조건
                         * 해당 좌표에 대각선인 상태로 놓이는 것은 이전 좌표의 방향이 어떻든 상관없으므로 모두 더해주고
                         * 대각선으로 놓이려면 인접한 곳도 이동가능한 지역이어야 하기 때문에 조건을 걸어준다.
                         */
                        if (map[i-1][j] == 0 && map[i][j-1] == 0) {
                            dp[i][j][2] = dp[i-1][j-1][0] + dp[i-1][j-1][1] + dp[i-1][j-1][2];
                        }
                    }
                }
            }
        }
        System.out.println(dp[N-1][N-1][0] + dp[N-1][N-1][1] + dp[N-1][N-1][2]);
    }
}
```


### 🔍 정답(BFS)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N;
    static int[][] status;
    static int[][] map;
    static int count = 0;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());

        status = new int[N][N];     // 파이프의 상태(가로: 1, 세로: 2, 대각선: 3)
        status[0][0] = status[0][1] = 1;

        map = new int[N][N];        // 집의 상태
        for (int i = 0; i < N; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        bfs(0, 1, 1);
        System.out.println(count);
    }

    public static void bfs(int x, int y, int stat) {
        Queue<Location> q = new LinkedList<>();
        q.offer(new Location(x, y, stat));

        while (!q.isEmpty()) {
            Location lo = q.poll();
            // 끝에 닿았다면 카운팅
            if (lo.x == N-1 && lo.y == N-1) count++;

            // 파이프 상태가 가로이거나 대각선일 경우,
            if (lo.status == 1 || lo.status == 3) {
                if (lo.y + 1 < N && map[lo.x][lo.y + 1] != 1) {
                    status[lo.x][lo.y + 1] = 1;
                    q.offer(new Location(lo.x, lo.y + 1, status[lo.x][lo.y + 1]));
                }
            }
            
            // 파이프 상태가 세로이거나 대각선일 경우,
            if (lo.status == 2 || lo.status == 3) {
                if (lo.x + 1 < N && map[lo.x + 1][lo.y] != 1) {
                    status[lo.x + 1][lo.y] = 2;
                    q.offer(new Location(lo.x + 1, lo.y, status[lo.x + 1][lo.y]));
                }
            }
            
            // 파이프 상태가 대각선일 경우,
            if (lo.x + 1 < N && lo.y + 1 < N && map[lo.x + 1][lo.y + 1] != 1 && map[lo.x][lo.y + 1] != 1 && map[lo.x + 1][lo.y] != 1) {
                status[lo.x + 1][lo.y + 1] = 3;
                q.offer(new Location(lo.x+1, lo.y + 1, status[lo.x + 1][lo.y + 1]));
            }
        }
    }

    public static class Location {
        int x, y, status;

        public Location(final int x, final int y, final int status) {
            this.x = x;
            this.y = y;
            this.status = status;
        }
    }
}
```


### 💡 정답(DFS)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N;
    static int[][] status;
    static int[][] map;
    static int count = 0;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());

        status = new int[N][N];     // 파이프의 상태(가로: 1, 세로: 2, 대각선: 3)
        status[0][0] = status[0][1] = 1;

        map = new int[N][N];        // 집의 상태
        for (int i = 0; i < N; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        dfs(0, 1, 1);
        System.out.println(count);
    }

    public static void dfs(int x, int y, int stat) {
        if (x == N - 1 && y == N - 1) {
            count++;
            return;
        }
        
        // 파이프 상태가 가로이거나 대각선일 경우,
        if (stat == 1 || stat == 3) {
            if (y + 1 < N && map[x][y + 1] != 1) {
                dfs(x, y+1, 1);
            }
        }

        // 파이프 상태가 세로이거 대각선일 경우,
        if (stat == 2 || stat == 3) {
            if (x + 1 < N && map[x + 1][y] != 1) {
                dfs(x+1, y, 2);
            }
        }

        // 파이프 상태가 대각선일 경우,
        if (x + 1 < N && y + 1 < N && map[x + 1][y + 1] != 1 && map[x][y + 1] != 1 && map[x + 1][y] != 1) {
            dfs(x+1, y+1, 3);
        }
    }
}
```


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FTgv9G%2Fbtr0do0zWhc%2Fc7JZReCRJaG25oERWzFxpk%2Fimg.png)

- 위부터 DP, DFS, BFS 풀이 결과이다.
- BFS가 익숙해서 BFS로 먼저 풀어보았는데 메모리 제한이 512MB인 문제에서 겨우 통과되었다. 
- DFS로도 풀어보니 거의 동일한 로직인데 차이가 엄청나는 것을 알 수 있다. 아마도 같은 좌표를 큐에 여러번 저장하고 빼다보니 성능이 떨어진 것 같다. BFS로 시간초과가 났다는 사람들도 여럿 있었다.
- 아마도 파이프 옮기기 시리즈로 나오는 문제인 것 같은데 다른 시리즈도 풀기 위해서는 성능상 이점도 챙겨가야 될 것 같기 때문에 굳이 완전 탐색이 필요하지 않다면 DP(맨 위)로 풀어보는 게 좋을 것 같다!