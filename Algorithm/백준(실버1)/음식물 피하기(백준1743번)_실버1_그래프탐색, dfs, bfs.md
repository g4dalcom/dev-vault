# [문제링크](https://www.acmicpc.net/problem/1743)

## 📝 문제

코레스코 콘도미니엄 8층은 학생들이 3끼의 식사를 해결하는 공간이다. 그러나 몇몇 비양심적인 학생들의 만행으로 음식물이 통로 중간 중간에 떨어져 있다. 이러한 음식물들은 근처에 있는 것끼리 뭉치게 돼서 큰 음식물 쓰레기가 된다. 

이 문제를 출제한 선생님은 개인적으로 이러한 음식물을 실내화에 묻히는 것을 정말 진정으로 싫어한다. 참고로 우리가 구해야 할 답은 이 문제를 낸 조교를 맞추는 것이 아니다. 

통로에 떨어진 음식물을 피해가기란 쉬운 일이 아니다. 따라서 선생님은 떨어진 음식물 중에 제일 큰 음식물만은 피해 가려고 한다. 

선생님을 도와 제일 큰 음식물의 크기를 구해서 “10ra"를 외치지 않게 도와주자.

## 입력

첫째 줄에 통로의 세로 길이 N(1 ≤ N ≤ 100)과 가로 길이 M(1 ≤ M ≤ 100) 그리고 음식물 쓰레기의 개수 K(1 ≤ K ≤ N×M)이 주어진다.  그리고 다음 K개의 줄에 음식물이 떨어진 좌표 (r, c)가 주어진다.

좌표 (r, c)의 r은 위에서부터, c는 왼쪽에서부터가 기준이다. 입력으로 주어지는 좌표는 중복되지 않는다.

## 출력

첫째 줄에 음식물 중 가장 큰 음식물의 크기를 출력하라.

## 예제 입력 1 

3 4 5
3 2
2 2
3 1
2 3
1 1

## 예제 출력 1

4

## 힌트

\# . . .
\. # # .
\# # . .

위와 같이 음식물이 떨어져있고 제일큰 음식물의 크기는 4가 된다. (인접한 것은 붙어서 크게 된다고 나와 있음. 대각선으로는 음식물 끼리 붙을수 없고 상하좌우로만 붙을수 있다.)

---

### 🔍 정답(BFS)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
    static int N, M, K;
    static int[][] map;
    static boolean[][] visit;
    static int[] dx = {0, 0, 1, -1};
    static int[] dy = {1, -1, 0, 0};

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());       // 통로의 세로 길이
        M = Integer.parseInt(st.nextToken());       // 통로의 가로 길이
        K = Integer.parseInt(st.nextToken());       // 쓰레기의 개수

        map = new int[N][M];
        visit = new boolean[N][M];

        // 쓰레기가 있는 좌표를 1로 표시, 인덱스는 0~N-1까지이기 때문에 입력값에 -1을 해줌!
        for (int i = 0; i < K; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken()) - 1;
            int b = Integer.parseInt(st.nextToken()) - 1;

            map[a][b] = 1;
        }

        int max = Integer.MIN_VALUE;
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                if (!visit[i][j] && map[i][j] == 1) {
                    visit[i][j] = true;
                    max = Math.max(max, bfs(i, j));
                }
            }
        }
        System.out.println(max);
    }

    public static int bfs(int x, int y) {
        Queue<Location> q = new LinkedList<>();
        visit[x][y] = true;
        q.offer(new Location(x, y));

        int cnt = 1;
        while (!q.isEmpty()) {
            Location lo = q.poll();

            for (int i = 0; i < 4; i++) {
                int cx = lo.x + dx[i];
                int cy = lo.y + dy[i];

                if (cx >= 0 && cy >= 0 && cx < N && cy < M) {
                    if (!visit[cx][cy] && map[cx][cy] == 1) {
                        visit[cx][cy] = true;
                        q.offer(new Location(cx, cy));
                        cnt++;
                    }
                }
            }
        }
        return cnt;
    }

    public static class Location {
        int x, y;

        public Location(final int x, final int y) {
            this.x = x;
            this.y = y;
        }
    }
}
```


### 🔎 정답(DFS)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    static int N, M, K;
    static int[][] map;
    static boolean[][] visit;
    static int[] dx = {0, 0, 1, -1};
    static int[] dy = {1, -1, 0, 0};
    static int max = Integer.MIN_VALUE;
    static int cnt;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());       // 통로의 세로 길이
        M = Integer.parseInt(st.nextToken());       // 통로의 가로 길이
        K = Integer.parseInt(st.nextToken());       // 쓰레기의 개수

        map = new int[N][M];
        visit = new boolean[N][M];

        // 쓰레기가 있는 좌표를 1로 표시, 인덱스는 0~N-1까지이기 때문에 입력값에 -1을 해줌!
        for (int i = 0; i < K; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken()) - 1;
            int b = Integer.parseInt(st.nextToken()) - 1;

            map[a][b] = 1;
        }

        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                if (!visit[i][j] && map[i][j] == 1) {
                    cnt = 1;
                    visit[i][j] = true;
                    dfs(i, j);
                }
            }
        }
        System.out.println(max);
    }

    public static void dfs(int x, int y) {

        for (int i = 0; i < 4; i++) {
            int cx = x + dx[i];
            int cy = y + dy[i];

            if (cx >= 0 && cy >= 0 && cx < N && cy < M) {
                if (!visit[cx][cy] && map[cx][cy] == 1) {
                    visit[cx][cy] = true;
                    cnt++;
                    dfs(cx, cy);
                }
            }
        }
        max = Math.max(max, cnt);
    }
}
```