# [문제링크](https://www.acmicpc.net/problem/15683)

## 📝 문제

스타트링크의 사무실은 1×1크기의 정사각형으로 나누어져 있는 N×M 크기의 직사각형으로 나타낼 수 있다. 사무실에는 총 K개의 CCTV가 설치되어져 있는데, CCTV는 5가지 종류가 있다. 각 CCTV가 감시할 수 있는 방법은 다음과 같다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcPHJDs%2FbtscBCgMQvm%2F8lvuksg16yOAQ2Hq1KVFMK%2Fimg.png)

1번 CCTV는 한 쪽 방향만 감시할 수 있다. 2번과 3번은 두 방향을 감시할 수 있는데, 2번은 감시하는 방향이 서로 반대방향이어야 하고, 3번은 직각 방향이어야 한다. 4번은 세 방향, 5번은 네 방향을 감시할 수 있다.

CCTV는 감시할 수 있는 방향에 있는 칸 전체를 감시할 수 있다. 사무실에는 벽이 있는데, CCTV는 벽을 통과할 수 없다. CCTV가 감시할 수 없는 영역은 사각지대라고 한다.

CCTV는 회전시킬 수 있는데, 회전은 항상 90도 방향으로 해야 하며, 감시하려고 하는 방향이 가로 또는 세로 방향이어야 한다.

0 0 0 0 0 0
0 0 0 0 0 0
0 0 1 0 6 0
0 0 0 0 0 0

지도에서 0은 빈 칸, 6은 벽, 1~5는 CCTV의 번호이다. 위의 예시에서 1번의 방향에 따라 감시할 수 있는 영역을 '`#`'로 나타내면 아래와 같다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcXBgo7%2FbtscKlxDMxf%2FqM8LeeJDa7vneJ0Aqp60DK%2Fimg.png)

CCTV는 벽을 통과할 수 없기 때문에, 1번이 → 방향을 감시하고 있을 때는 6의 오른쪽에 있는 칸을 감시할 수 없다.

0 0 0 0 0 0
0 2 0 0 0 0
0 0 0 0 6 0
0 6 0 0 2 0
0 0 0 0 0 0
0 0 0 0 0 5

위의 예시에서 감시할 수 있는 방향을 알아보면 아래와 같다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlT3Mb%2FbtscJqy4dAx%2FPVcKLqApQtYlz1XXReqpnk%2Fimg.png)

CCTV는 CCTV를 통과할 수 있다. 아래 예시를 보자.

0 0 2 0 3
0 6 0 0 0
0 0 6 6 0
0 0 0 0 0

위와 같은 경우에 2의 방향이 ↕ 3의 방향이 ←와 ↓인 경우 감시받는 영역은 다음과 같다.

\# # 2 # 3
0 6 # 0 #
0 0 6 6 #
0 0 0 0 #

사무실의 크기와 상태, 그리고 CCTV의 정보가 주어졌을 때, CCTV의 방향을 적절히 정해서, 사각 지대의 최소 크기를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 사무실의 세로 크기 N과 가로 크기 M이 주어진다. (1 ≤ N, M ≤ 8)

둘째 줄부터 N개의 줄에는 사무실 각 칸의 정보가 주어진다. 0은 빈 칸, 6은 벽, 1~5는 CCTV를 나타내고, 문제에서 설명한 CCTV의 종류이다. 

CCTV의 최대 개수는 8개를 넘지 않는다.

## 출력

첫째 줄에 사각 지대의 최소 크기를 출력한다.

## 예제 입력 1 

4 6
0 0 0 0 0 0
0 0 0 0 0 0
0 0 1 0 6 0
0 0 0 0 0 0

## 예제 출력 1 

20

## 예제 입력 2 

6 6
0 0 0 0 0 0
0 2 0 0 0 0
0 0 0 0 6 0
0 6 0 0 2 0
0 0 0 0 0 0
0 0 0 0 0 5

## 예제 출력 2

15

## 예제 입력 3 

6 6
1 0 0 0 0 0
0 1 0 0 0 0
0 0 1 0 0 0
0 0 0 1 0 0
0 0 0 0 1 0
0 0 0 0 0 1

## 예제 출력 3 

6

## 예제 입력 4 

6 6
1 0 0 0 0 0
0 1 0 0 0 0
0 0 1 5 0 0
0 0 5 1 0 0
0 0 0 0 1 0
0 0 0 0 0 1

## 예제 출력 4 

2

## 예제 입력 5 
1 7
0 1 2 3 4 5 6

## 예제 출력 5 

0

## 예제 입력 6

3 7
4 0 0 0 0 0 0
0 0 0 2 0 0 0
0 0 0 0 0 0 4

## 예제 출력 6 

0

---

### 🔍 정답

```java
import java.util.*;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    static int N, M;
    static int[][] map;
    static ArrayList<Location> locations;
    static int spot = 0;
    static int[] dx = {-1, 0, 1, 0};    // 상 우 하 좌
    static int[] dy = {0, 1, 0, -1};
    static boolean[][] visit;
    static int min = Integer.MAX_VALUE;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        map = new int[N][M];                // 입력값으로 구현한 맵
        locations = new ArrayList<>();      // CCTV가 있는 좌표를 저장할 리스트

        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < M; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());

                // 빈 칸인 지역은 카운팅해줌. 이후에 CCTV가 감시하는 지역을 빼서 사각지대의 개수를 구하기 위함
                if (map[i][j] == 0) spot++;
                else if (map[i][j] > 0 && map[i][j] < 6) {
                    locations.add(new Location(i, j));
                }
            }
        }

        dfs(0, new Location[locations.size()]);

        System.out.println(min);
    }

    public static void dfs(int idx, Location[] cctvs) {

        // 백트래킹 종료 조건!
        if (idx == locations.size()) {
            blindSpot(cctvs);
        }

        else {
            // CCTV가 있는 구역을 하나씩 꺼냄
            Location lo = locations.get(idx);

            // 케이스별로 방향을 저장!
            for (int i = 0; i < 4; i++) {
                Location c = new Location(lo.x, lo.y);

                switch (map[c.x][c.y]) {
                    case 1:
                        c.addDirection(i);
                        cctvs[idx] = c;
                        dfs(idx + 1, cctvs);
                        break;

                    case 2:
                        if (i >= 2) return;
                        c.addDirection(i);
                        c.addDirection(i+2);
                        cctvs[idx] = c;
                        dfs(idx + 1, cctvs);
                        break;

                    case 3:
                        c.addDirection(i);
                        c.addDirection((i+1) % 4);
                        cctvs[idx] = c;
                        dfs(idx + 1, cctvs);
                        break;

                    case 4:
                        c.addDirection(i);
                        c.addDirection((i+1) % 4);
                        c.addDirection((i+2) % 4);
                        cctvs[idx] = c;
                        dfs(idx + 1, cctvs);
                        break;

                    case 5:
                        if (i >= 1) return;
                        c.addDirection(i);
                        c.addDirection((i+1) % 4);
                        c.addDirection((i+2) % 4);
                        c.addDirection((i+3) % 4);
                        cctvs[idx] = c;
                        dfs(idx + 1, cctvs);
                        break;
                }
            }
        }
    }

    public static void blindSpot(Location[] cctvs) {
        visit = new boolean[N][M];

        int cnt = 0;
        for (int i = 0; i < cctvs.length; i++) {
            Location cctv = cctvs[i];

            // 방향(d)을 꺼내서 맵 범위내 + 벽을 만나지 않는 조건하에서 감시 구역을 카운팅
            for (int j = 0; j < cctv.getDirectionSize(); j++) {
                int d = cctv.directions.get(j);
                int cx = cctv.x + dx[d];
                int cy = cctv.y + dy[d];

                while (cx >= 0 && cx < N && cy >= 0 && cy < M) {
                    if (map[cx][cy] == 6) break;

                    if (map[cx][cy] == 0 && !visit[cx][cy]) {
                        visit[cx][cy] = true;
                        cnt++;
                    }

                    cx += dx[d];
                    cy += dy[d];
                }
            }
        }
        min = Math.min(spot - cnt, min);

    }

    public static class Location {
        int x, y;
        ArrayList<Integer> directions = new ArrayList<>();

        public Location(int x, int y) {
            this.x = x;
            this.y = y;
        }

        public void addDirection(int direction) {
            directions.add(direction);
        }

        public int getDirectionSize() {
            return directions.size();
        }
    }
}
```
- [참고 사이트](https://maivve.tistory.com/101)
- 어떻게 구현해야 될지 몰라서 다른 사이트들을 참고했다.
- 참고해서 풀고나면 뭔가 알 거 같은 느낌인데 조금만 응용해서 나와도 헤맬 것 같다 ㅠㅠ
- 이런 문제들은 체크해두었다가 수시로 다시 풀어보면서 감을 익혀야 할 거 같다!