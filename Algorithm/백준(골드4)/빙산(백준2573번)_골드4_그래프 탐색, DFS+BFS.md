# [문제링크](https://www.acmicpc.net/problem/2573)

## 📝 문제

지구 온난화로 인하여 북극의 빙산이 녹고 있다. 빙산을 그림 1과 같이 2차원 배열에 표시한다고 하자. 빙산의 각 부분별 높이 정보는 배열의 각 칸에 양의 정수로 저장된다. 빙산 이외의 바다에 해당되는 칸에는 0이 저장된다. 그림 1에서 빈칸은 모두 0으로 채워져 있다고 생각한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlXzLy%2FbtsagsnaMMc%2FvuYFg4si2Z8PiPfxPCoOG1%2Fimg.png)
 
그림 1. 행의 개수가 5이고 열의 개수가 7인 2차원 배열에 저장된 빙산의 높이 정보

빙산의 높이는 바닷물에 많이 접해있는 부분에서 더 빨리 줄어들기 때문에, 배열에서 빙산의 각 부분에 해당되는 칸에 있는 높이는 일년마다 그 칸에 동서남북 네 방향으로 붙어있는 0이 저장된 칸의 개수만큼 줄어든다. 단, 각 칸에 저장된 높이는 0보다 더 줄어들지 않는다. 바닷물은 호수처럼 빙산에 둘러싸여 있을 수도 있다. 따라서 그림 1의 빙산은 일년후에 그림 2와 같이 변형된다.

그림 3은 그림 1의 빙산이 2년 후에 변한 모습을 보여준다. 2차원 배열에서 동서남북 방향으로 붙어있는 칸들은 서로 연결되어 있다고 말한다. 따라서 그림 2의 빙산은 한 덩어리이지만, 그림 3의 빙산은 세 덩어리로 분리되어 있다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcgXd3z%2FbtsaqQs8aux%2FUKNLntSKkmzmgwKDCPKCbk%2Fimg.png)

한 덩어리의 빙산이 주어질 때, 이 빙산이 두 덩어리 이상으로 분리되는 최초의 시간(년)을 구하는 프로그램을 작성하시오. 그림 1의 빙산에 대해서는 2가 답이다. 만일 전부 다 녹을 때까지 두 덩어리 이상으로 분리되지 않으면 프로그램은 0을 출력한다.

## 입력

첫 줄에는 이차원 배열의 행의 개수와 열의 개수를 나타내는 두 정수 N과 M이 한 개의 빈칸을 사이에 두고 주어진다. N과 M은 3 이상 300 이하이다. 그 다음 N개의 줄에는 각 줄마다 배열의 각 행을 나타내는 M개의 정수가 한 개의 빈 칸을 사이에 두고 주어진다. 각 칸에 들어가는 값은 0 이상 10 이하이다. 배열에서 빙산이 차지하는 칸의 개수, 즉, 1 이상의 정수가 들어가는 칸의 개수는 10,000 개 이하이다. 배열의 첫 번째 행과 열, 마지막 행과 열에는 항상 0으로 채워진다.

## 출력

첫 줄에 빙산이 분리되는 최초의 시간(년)을 출력한다. 만일 빙산이 다 녹을 때까지 분리되지 않으면 0을 출력한다.

## 예제 입력 1 복사

5 7
0 0 0 0 0 0 0
0 2 4 5 3 0 0
0 3 0 2 5 2 0
0 7 6 2 4 0 0
0 0 0 0 0 0 0

## 예제 출력 1 복사

2

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N, M;
    static int[][] map;
    static boolean[][] visit;
    static int[] dx = {0, 0, 1, -1};
    static int[] dy = {1, -1, 0, 0};
    static int year = 0;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        map = new int[N][M];

        // 빙하 높이 입력
        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < M; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        while (true) {

            // 빙하 덩어리를 카운팅
            int cnt = 0;
            visit = new boolean[N][M];

            for (int i = 0; i < N; i++) {
                for (int j = 0; j < M; j++) {
                    if (map[i][j] != 0 && !visit[i][j]) {
                        checkDivision(i, j);
                        cnt++;
                    }
                }
            }
            
            if (cnt == 0) {
                System.out.println(0);
                return;
            } else if (cnt >= 2) {
                System.out.println(year);
                return;
            } else {
                year++;
                melting();
            }
        }
    }

    public static void melting() {
        Queue<Location> q = new LinkedList<>();
        boolean[][] melted = new boolean[N][M];

        // 빙하 지역을 큐에 넣기
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                if (map[i][j] != 0) {
                    melted[i][j] = true;
                    q.offer(new Location(i, j));
                }
            }
        }

        // 주변의 바다인 곳의 개수를 세고 높이에서 그 수만큼 빼는 로직
        while (!q.isEmpty()) {
            Location lo = q.poll();

            int around = 0;
            for (int i = 0; i < 4; i++) {
                int cx = lo.x + dx[i];
                int cy = lo.y + dy[i];

                if (cx >= 0 && cy >= 0 && cx < N && cy < M) {
                    if (map[cx][cy] == 0 && !melted[cx][cy]) {
                        around++;
                    }
                }
            }
            if (map[lo.x][lo.y] - around < 0) map[lo.x][lo.y] = 0;
            else map[lo.x][lo.y] -= around;
        }
    }

    // 하나로 이어진 빙하지역 구하기
    public static void checkDivision(int x, int y) {
        visit[x][y] = true;

        for (int i = 0; i < 4; i++) {
            int cx = x + dx[i];
            int cy = y + dy[i];

            if (cx >= 0 && cy >= 0 && cx < N && cy < M) {
                if (map[cx][cy] != 0 && !visit[cx][cy]) {
                    checkDivision(cx, cy);
                }
            }
        }
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
- 먼저 빙하 지역의 개수를 세어야 하고, 1년이 지났을 때 빙하 주변 바다 타일 개수에 따른 높이 변화를 구해야 하는 두 가지 과제가 있다.
- 먼저 빙하 지역의 개수를 세는 것은 DFS를 이용해서 하나의 지역을 모두 탐색했을 때 cnt+1 을 하도록 하였다.

```java
int cnt = 0;
visit = new boolean[N][M];

for (int i = 0; i < N; i++) {
	for (int j = 0; j < M; j++) {
		if (map[i][j] != 0 && !visit[i][j]) {
			checkDivision(i, j);
			cnt++;
		}
	}
}
```

- 그리고 구해진 cnt 에 따라, 빙하가 모두 녹아서 cnt == 0이라면 0을 리턴하고, cnt 가 2 이상이라면 연수(year)를 반환하고, cnt == 1 이라면 빙하를 녹이는 작업을 하도록 했다!

```java
if (cnt == 0) {
	System.out.println(0);
	return;
} else if (cnt >= 2) {
	System.out.println(year);
	return;
} else {
	year++;
	melting();
}
```

- 빙하를 녹이는 것은 모든 좌표를 탐색할 필요없이 빙하인 곳만 탐색하면 되기 때문에 Queue에 빙하인 곳을 넣고 하나씩 poll() 하면서 주변 바다의 개수를 세었다.
- 여기서 중요했던 것은, 빙하인 지역이 map\[1]\[1] 과 map\[1]\[2] 일 때, map\[1]\[1]을 먼저 탐색해서 높이가 0이 되면 map\[1]\[2]는 그 지역까지 바다로 계산해서 의도했던 것보다 높이가 더 줄어버리는 경우가 있을 수 있다. 
- 그래서 이것을 방지하기 위해 melted라는 boolean배열에 빙하인 지역을 true로 넣어서 주변 바다를 탐색할 때 빙하인 지역은 탐색하지 않도록 했다!

```java
for (int i = 0; i < N; i++) {
	for (int j = 0; j < M; j++) {
		if (map[i][j] != 0) {
			melted[i][j] = true;
			q.offer(new Location(i, j));
		}
	}
}

						.
						.
						.


Location lo = q.poll();

int around = 0;
for (int i = 0; i < 4; i++) {
	int cx = lo.x + dx[i];
	int cy = lo.y + dy[i];

	if (cx >= 0 && cy >= 0 && cx < N && cy < M) {
		if (map[cx][cy] == 0 && !melted[cx][cy]) {
			around++;
		}
	}
}

```