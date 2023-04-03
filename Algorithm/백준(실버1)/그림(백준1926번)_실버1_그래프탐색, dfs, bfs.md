# [문제링크](https://www.acmicpc.net/problem/1926)

## 📝 문제

어떤 큰 도화지에 그림이 그려져 있을 때, 그 그림의 개수와, 그 그림 중 넓이가 가장 넓은 것의 넓이를 출력하여라. 단, 그림이라는 것은 1로 연결된 것을 한 그림이라고 정의하자. 가로나 세로로 연결된 것은 연결이 된 것이고 대각선으로 연결이 된 것은 떨어진 그림이다. 그림의 넓이란 그림에 포함된 1의 개수이다.

## 입력

첫째 줄에 도화지의 세로 크기 n(1 ≤ n ≤ 500)과 가로 크기 m(1 ≤ m ≤ 500)이 차례로 주어진다. 두 번째 줄부터 n+1 줄 까지 그림의 정보가 주어진다. (단 그림의 정보는 0과 1이 공백을 두고 주어지며, 0은 색칠이 안된 부분, 1은 색칠이 된 부분을 의미한다)

## 출력

첫째 줄에는 그림의 개수, 둘째 줄에는 그 중 가장 넓은 그림의 넓이를 출력하여라. 단, 그림이 하나도 없는 경우에는 가장 넓은 그림의 넓이는 0이다.

## 예제 입력 1

6 5
1 1 0 1 1
0 1 1 0 0
0 0 0 0 0
1 0 1 1 1
0 0 1 1 1
0 0 1 1 1

## 예제 출력 1 

4
9

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
    static int n, m;
    static int[][] map;
    static boolean[][] visit;
    static int dx[] = {0, 0, 1, -1};
    static int dy[] = {1, -1, 0, 0};
    static int size = 0;
    static int max = 0;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        n = Integer.parseInt(st.nextToken());   // 도화지 세로 크기
        m = Integer.parseInt(st.nextToken());   // 도화지 가로 크기

        map = new int[n][m];
        visit = new boolean[n][m];

        for (int i = 0; i < n; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < m; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }
        
        int cnt = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (!visit[i][j] && map[i][j] == 1) {
                    bfs(i, j);
                    cnt++;
                }
            }
        }
        System.out.println(cnt + "\n" + max);
    }

    public static void bfs(int x, int y) {
        Queue<Location> q = new LinkedList<>();
        visit[x][y] = true;
        q.offer(new Location(x, y));

        size = 1;
        while (!q.isEmpty()) {
            Location lo = q.poll();

            for (int i = 0; i < 4; i++) {
                int cx = lo.x + dx[i];
                int cy = lo.y + dy[i];

                if (cx >= 0 && cy >= 0 && cx < n && cy < m) {
                    if (!visit[cx][cy] && map[cx][cy] == 1) {
                        visit[cx][cy] = true;
                        q.offer(new Location(cx, cy));
                        size++;
                    }
                }
            }
        }
        max = Math.max(max, size);
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
    static int n, m;
    static int[][] map;
    static boolean[][] visit;
    static int dx[] = {0, 0, 1, -1};
    static int dy[] = {1, -1, 0, 0};
    static int size = 0;
    static int max = 0;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        n = Integer.parseInt(st.nextToken());   // 도화지 세로 크기
        m = Integer.parseInt(st.nextToken());   // 도화지 가로 크기

        map = new int[n][m];
        visit = new boolean[n][m];

        for (int i = 0; i < n; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < m; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        int cnt = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (!visit[i][j] && map[i][j] == 1) {
                    size = 1;
                    dfs(i, j);
                    cnt++;
                }
            }
        }
        System.out.println(cnt + "\n" + max);
    }

    public static void dfs(int x, int y) {
        visit[x][y] = true;
        max = Math.max(max, size);

        for (int i = 0; i < 4; i++) {
            int cx = x + dx[i];
            int cy = y + dy[i];

            if (cx >= 0 && cy >= 0 && cx < n && cy < m) {
                if (!visit[cx][cy] && map[cx][cy] == 1) {
                    size++;
                    dfs(cx, cy);
                }
            }
        }
    }
}
```
- 매우 일반적인 그래프 탐색 알고리즘 문제였다.
- 연습할 겸 BFS와 DFS 모두 사용해서 풀어보았다~~