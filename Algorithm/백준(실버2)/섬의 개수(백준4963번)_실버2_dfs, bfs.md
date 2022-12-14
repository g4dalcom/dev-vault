# [문제링크](https://www.acmicpc.net/problem/4963)

## 📝 문제

정사각형으로 이루어져 있는 섬과 바다 지도가 주어진다. 섬의 개수를 세는 프로그램을 작성하시오.

![](https://www.acmicpc.net/upload/images/island.png)

한 정사각형과 가로, 세로 또는 대각선으로 연결되어 있는 사각형은 걸어갈 수 있는 사각형이다. 

두 정사각형이 같은 섬에 있으려면, 한 정사각형에서 다른 정사각형으로 걸어서 갈 수 있는 경로가 있어야 한다. 지도는 바다로 둘러싸여 있으며, 지도 밖으로 나갈 수 없다.

## 입력

입력은 여러 개의 테스트 케이스로 이루어져 있다. 각 테스트 케이스의 첫째 줄에는 지도의 너비 w와 높이 h가 주어진다. w와 h는 50보다 작거나 같은 양의 정수이다.

둘째 줄부터 h개 줄에는 지도가 주어진다. 1은 땅, 0은 바다이다.

입력의 마지막 줄에는 0이 두 개 주어진다.

## 출력

각 테스트 케이스에 대해서, 섬의 개수를 출력한다.

## 예제 입력 1 

1 1
0
2 2
0 1
1 0
3 2
1 1 1
1 1 1
5 4
1 0 1 0 0
1 0 0 0 0
1 0 1 0 1
1 0 0 1 0
5 4
1 1 1 0 1
1 0 1 0 1
1 0 1 0 1
1 0 1 1 1
5 5
1 0 1 0 1
0 0 0 0 0
1 0 1 0 1
0 0 0 0 0
1 0 1 0 1
0 0

## 예제 출력 1 

0
1
1
3
1
9


---

### 🔍 정답

### DFS

```java
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
import java.util.*;  
  
public class Main {  
    static int w, h;  
    static int[][] islands;  
    static boolean[][] visit;  
  
    public static void main(String[] args) throws IOException {  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        StringBuilder sb = new StringBuilder();  
  
        while (true) {  
            StringTokenizer st = new StringTokenizer(br.readLine());  
            w = Integer.parseInt(st.nextToken()); // 지도의 너비  
            h = Integer.parseInt(st.nextToken()); // 지도의 높이  
  
            /* 입력값이 0 0 이면 입력 종료! */  
            if (w == 0 && h == 0) break;  
  
            /* 섬의 좌표를 입력받을 배열 */            islands = new int[h][w];  
            visit = new boolean[h][w];  
  
            /* 입력 받기 */            for (int i = 0; i < h; i++) {  
                st = new StringTokenizer(br.readLine());  
                for (int j = 0; j < w; j++) {  
                    islands[i][j] = Integer.parseInt(st.nextToken());  
                }  
            }  
  
            /**  
             * [0, 0] 부터 시작해서 섬이면서 방문하지 않았던 노드라면 dfs 함수를 타고,  
             * dfs 함수에서 이어진 모든 곳을 탐색한 후 나오면 카운팅을 한다  
             * dfs 함수 내에서 이어진 곳들을 탐색하면서 visit = true를 해주기 때문에  
             * 다음 dfs함수를 탈 때는 이전에 탐색한 곳과 이어지지 않은 곳부터 다시 탐색하게 될 것이다!  
             */            int cnt = 0;  
            for (int i = 0; i < h; i++) {  
                for (int j = 0; j < w; j++) {  
                    if (islands[i][j] == 1 && !visit[i][j]) {  
                        cnt++;  
                        dfs(i, j);  
                    }  
                }  
            }  
            sb.append(cnt).append("\n");  
        }  
        System.out.println(sb);  
    }  
  
    public static void dfs(int x, int y) {  
        /* 주변에 이어진 곳을 탐색하기 위한 좌표 */        int[] cx = {0, 0, -1, 1, 1, 1, -1, -1};  
        int[] cy = {1, -1, 0, 0, 1, -1, -1, 1};  
  
        visit[x][y] = true;  
  
        /* 해당 좌표에서 상하좌우대각선까지 이어진 곳을 파악하기 위해 좌표를 도는 로직 */        for (int i = 0; i < cx.length; i++) {  
            int dx = x + cx[i];  
            int dy = y + cy[i];  
  
            /* 탐색하는 곳이 지도 안쪽이면서, 섬이고, 방문하지 않았다면 그 좌표로 dfs 재귀 호출! */  
            if (dx >= 0 && dy >= 0 && dx < h && dy < w) {  
                if (islands[dx][dy] == 1 && !visit[dx][dy]) {  
                    dfs(dx, dy);  
                }  
            }  
        }  
    }  
}
```

### BFS

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int w, h;
    static int[][] islands;
    static boolean[][] visit;
    static int[] cx = {0, 0, -1, 1, 1, 1, -1, -1};
    static int[] cy = {1, -1, 0, 0, 1, -1, -1, 1};
    static class Node {
        int x, y;
        public Node(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();

        while (true) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            w = Integer.parseInt(st.nextToken()); // 지도의 너비
            h = Integer.parseInt(st.nextToken()); // 지도의 높이

            /* 입력값이 0 0 이면 입력 종료! */
            if (w == 0 && h == 0) break;

            /* 섬의 좌표를 입력받을 배열 */
            islands = new int[h][w];
            visit = new boolean[h][w];

            /* 입력 받기 */
            for (int i = 0; i < h; i++) {
                st = new StringTokenizer(br.readLine());
                for (int j = 0; j < w; j++) {
                    islands[i][j] = Integer.parseInt(st.nextToken());
                }
            }

            /**
             * [0, 0] 부터 시작해서 섬이면서 방문하지 않았던 노드라면 dfs 함수를 타고,
             * bfs 함수에서 이어진 모든 곳을 탐색한 후 나오면 카운팅을 한다
             * bfs 함수 내에서 이어진 곳들을 탐색하면서 visit = true를 해주기 때문에
             * 다음 bfs함수를 탈 때는 이전에 탐색한 곳과 이어지지 않은 곳부터 다시 탐색하게 될 것이다!
             */
            int cnt = 0;
            for (int i = 0; i < h; i++) {
                for (int j = 0; j < w; j++) {
                    if (islands[i][j] == 1 && !visit[i][j]) {
                        cnt++;
                        bfs(i, j);
                    }
                }
            }
            sb.append(cnt).append("\n");
        }
        System.out.println(sb);
    }

    public static void bfs(int x, int y) {
	    /* 큐 선언하고 초기값 넣어주기 */
        Queue<Node> q = new LinkedList<>();
        q.offer(new Node(x, y));

        visit[x][y] = true;

        while (!q.isEmpty()) {
			/* 큐에 있는 것을 순차적으로 빼준다! */
            Node node = q.poll();

			/* 상하좌우대각선까지 이어진 곳 탐색 */
            for (int i = 0; i < cx.length; i++) {
                int dx = node.x + cx[i];
                int dy = node.y + cy[i];

				/**
				* dfs처럼 중간에 재귀호출을 하지 않고 주변 모든 곳을 탐색하면서
				* 지도 범위 내에 있고, 섬이고, 방문하지 않은 곳이라면 큐에 차례대로 넣어준다!
				* 큐가 빌 때까지 주변의 주변의 주변을 돌면서 이어진 곳들을 모두 발견할 것이다!
				*/
                if (dx >= 0 && dy >= 0 && dx < h && dy < w) {
                    if (islands[dx][dy] == 1 && !visit[dx][dy]) {
                        visit[dx][dy] = true;
                        q.offer(new Node(dx, dy));
                    }
                }
            }
        }
    }
}
```