# [문제링크](https://www.acmicpc.net/problem/1916)

## 📝 문제

N개의 도시가 있다. 그리고 한 도시에서 출발하여 다른 도시에 도착하는 M개의 버스가 있다. 우리는 A번째 도시에서 B번째 도시까지 가는데 드는 버스 비용을 최소화 시키려고 한다. A번째 도시에서 B번째 도시까지 가는데 드는 최소비용을 출력하여라. 도시의 번호는 1부터 N까지이다.

## 입력

첫째 줄에 도시의 개수 N(1 ≤ N ≤ 1,000)이 주어지고 둘째 줄에는 버스의 개수 M(1 ≤ M ≤ 100,000)이 주어진다. 그리고 셋째 줄부터 M+2줄까지 다음과 같은 버스의 정보가 주어진다. 먼저 처음에는 그 버스의 출발 도시의 번호가 주어진다. 그리고 그 다음에는 도착지의 도시 번호가 주어지고 또 그 버스 비용이 주어진다. 버스 비용은 0보다 크거나 같고, 100,000보다 작은 정수이다.

그리고 M+3째 줄에는 우리가 구하고자 하는 구간 출발점의 도시번호와 도착점의 도시번호가 주어진다. 출발점에서 도착점을 갈 수 있는 경우만 입력으로 주어진다.

## 출력

첫째 줄에 출발 도시에서 도착 도시까지 가는데 드는 최소 비용을 출력한다.

## 예제 입력 1

5
8
1 2 2
1 3 3
1 4 1
1 5 10
2 4 2
3 4 1
3 5 1
4 5 3
1 5

## 예제 출력 1 

4

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class Main {
    static int N, M;
    static ArrayList<ArrayList<Node>> info;
    static int[] dp;
    static boolean[] visit;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());        // 도시의 개수
        M = Integer.parseInt(br.readLine());        // 버스의 개수

        visit = new boolean[N+1];                   // 방문 여부 체크할 배열
        dp = new int[N+1];                        // 각 노드별 최소 비용을 저장
        Arrays.fill(dist, Integer.MAX_VALUE);

        info = new ArrayList<>();
        for (int i = 0; i <= N; i++) {
            info.add(new ArrayList<>());
        }

        StringTokenizer st;

        /**
         * 각 List에 도착 노드와 비용을 저장한다.
         * List[1] = [2, 2] [3, 3] [4, 1] [5, 10] 이 저장된다.
         */
        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());
            int start = Integer.parseInt(st.nextToken());
            int end = Integer.parseInt(st.nextToken());
            int fare = Integer.parseInt(st.nextToken());

            info.get(start).add(new Node(end, fare));
        }

        st = new StringTokenizer(br.readLine());
        int start = Integer.parseInt(st.nextToken());
        int destination = Integer.parseInt(st.nextToken());

        System.out.println(dijkstra(start, destination));
    }

    public static int dijikstra(int start, int destination) {
        PriorityQueue<Node> pq = new PriorityQueue<>();
        pq.offer(new Node(start, 0));
        dp[start] = 0;        // 시작 노드부터 탐색!

        while (!pq.isEmpty()) {
            Node current = pq.poll();
            int curEnd = current.end;

            /**
             * 현재 노드의 도착 지점들을 하나씩 꺼내서
             * dp[도착 지점]에 도착 지점의 최소비용을 갱신한다.
             * 1의 도착 지점인 2, 3, 4, 5의 각 도착 지점들(2의 경우 4, 3의 경우 4, 5)의 비용과
             * 현재 노드까지의 비용 + 해당 도착 지점으로 갈 때 드는 비용을 비교해서 더 작은 값을 저장한다.
             * 
             * 5의 경우,
             * 처음 1의 도착 지점으로 선택되었을 때 dp[1] + 10(1 -> 5의 fare) 로 dp[5] = 10 이 된다.
             * 그리고 3의 도착 지점으로 선택되었을 때 dp[3] + 1(3 -> 5의 fare) 로 dp[5] = 4로 갱신이 된다.
             */
            if (!visit[curEnd]) {
                visit[curEnd] = true;

                for (Node node : info.get(curEnd)) {
                    if (!visit[node.end] && dp[node.end] > dp[curEnd] + node.fare) {
                        dp[node.end] = dp[curEnd] + node.fare;
                        pq.add(new Node(node.end, dp[node.end]));
                    }
                }
            }
        }
        return dp[destination];
    }

    public static class Node implements Comparable<Node> {
        int end, fare;

        public Node(final int end, final int fare) {
            this.end = end;
            this.fare = fare;
        }

        /**
         * 같은 경로이지만 비용이 다른 경우에는 더 적은 비용으로 갱신하기 위함!
         */
        @Override
        public int compareTo(final Node o) {
            return fare - o.fare;
        }
    }
}
```
- 다익스트라 알고리즘의 기본 문제라고 한다.
- 다익스트라는 하나의 정점에서 다른 모든 정점으로 가는 최단 거리를 계산하는 `단일 시작점 경로 알고리즘`으로 모든 정점에서 다른 모든 정점으로 가는 최단 거리를 계산하는 플로이드 워셜과 비슷하면서 다르다.
- 기준 노드 하나를 잡고 연결된 노드들을 방문하면서 가장 적은 비용을 dp배열에 갱신해간다.

### ❌ 오답 (처음 풀이, dfs)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    static int N, M;
    static int[][] bus;
    static boolean[] visit;
    static int min = Integer.MAX_VALUE;
    static int count = 0;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());        // 도시의 개수
        M = Integer.parseInt(br.readLine());        // 버스의 개수

        bus = new int[N+1][N+1];
        visit = new boolean[N+1];

        for (int i = 0; i < M; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            bus[Integer.parseInt(st.nextToken())][Integer.parseInt(st.nextToken())] = Integer.parseInt(st.nextToken());
        }

        StringTokenizer st = new StringTokenizer(br.readLine());
        int start = Integer.parseInt(st.nextToken());
        int end = Integer.parseInt(st.nextToken());

        dfs(start, end);
        System.out.println(min);
    }

    public static void dfs(int start, int destination) {
        if (start == destination) {
            min = Math.min(min, count);
            return;
        }

        for (int i = 1; i <= N; i++) {
            if (bus[start][i] != 0) {
                count += bus[start][i];
                dfs(i, destination);
                count -= bus[start][i];
            }
        }
    }
}
```
- 버스의 출발점과 도착점을 2차원 배열로 저장하고 좌표값을 비용으로 설정하고 연결되어있지 않은 좌표는 0으로 설정하였다.
- 그리고 재귀를 부르며 카운팅을 하고 start 지점이 도착지점이 될 때 카운트의 값을 최솟값으로 갱신하도록 하였다.
- 그런데 반례 중 각 노드간 비용이 0인 것도 있고 그래프 탐색 알고리즘으로 풀면 시간초과가 나서 다익스트라 알고리즘을 이용해야 한다고 한다 ㅠㅠ
