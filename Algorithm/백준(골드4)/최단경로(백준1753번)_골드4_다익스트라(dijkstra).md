# [문제링크](https://www.acmicpc.net/problem/1753)

## 📝 문제

방향그래프가 주어지면 주어진 시작점에서 다른 모든 정점으로의 최단 경로를 구하는 프로그램을 작성하시오. 단, 모든 간선의 가중치는 10 이하의 자연수이다.

## 입력

첫째 줄에 정점의 개수 V와 간선의 개수 E가 주어진다. (1 ≤ V ≤ 20,000, 1 ≤ E ≤ 300,000) 모든 정점에는 1부터 V까지 번호가 매겨져 있다고 가정한다. 둘째 줄에는 시작 정점의 번호 K(1 ≤ K ≤ V)가 주어진다. 셋째 줄부터 E개의 줄에 걸쳐 각 간선을 나타내는 세 개의 정수 (u, v, w)가 순서대로 주어진다. 이는 u에서 v로 가는 가중치 w인 간선이 존재한다는 뜻이다. u와 v는 서로 다르며 w는 10 이하의 자연수이다. 서로 다른 두 정점 사이에 여러 개의 간선이 존재할 수도 있음에 유의한다.

## 출력

첫째 줄부터 V개의 줄에 걸쳐, i번째 줄에 i번 정점으로의 최단 경로의 경로값을 출력한다. 시작점 자신은 0으로 출력하고, 경로가 존재하지 않는 경우에는 INF를 출력하면 된다.

## 예제 입력 1 

5 6
1
5 1 1
1 2 2
1 3 3
2 3 4
2 4 5
3 4 6

## 예제 출력 1 

0
2
3
7
INF

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
    static int V, E, K;
    static ArrayList<Node>[] node;
    static int[] dp;
    static boolean[] visit;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        V = Integer.parseInt(st.nextToken());       // 정점의 개수
        E = Integer.parseInt(st.nextToken());       // 간선의 개수

        K = Integer.parseInt(br.readLine());        // 시작점 번호

        node = new ArrayList[V+1];
        for (int i = 1; i <= V; i++) {
            node[i] = new ArrayList<>();
        }

        /**
         * 정점별로 이어진 도착점과 비용을 담아둔다!
         */
        for (int i = 0; i < E; i++) {
            st = new StringTokenizer(br.readLine());
            int start = Integer.parseInt(st.nextToken());
            int end = Integer.parseInt(st.nextToken());
            int fare = Integer.parseInt(st.nextToken());

            node[start].add(new Node(end, fare));
        }

        dijkstra(K);

        for (int i = 1; i < dp.length; i++) {
            if (dp[i] == Integer.MAX_VALUE)
                System.out.println("INF");
            else
                System.out.println(dp[i]);
        }

    }

    public static void dijkstra(int start) {
        dp = new int[V+1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[start] = 0;
        visit = new boolean[V+1];

        PriorityQueue<Node> pq = new PriorityQueue<>();
        pq.offer(new Node(start, 0));

        while (!pq.isEmpty()) {
            Node cur = pq.poll();
            int curEnd = cur.end;

            /**
             * 시작 노드에서 출발해서 curEnd를 거쳐서 갈 수 있는 (curEnd와 이어진 간선들) 곳들을 탐색
             * 새로운 경로의 가중치와 기존 가중치를 비교해서 더 적은 값을 갱신!
             */
            for (Node node : node[curEnd]) {
                if (dp[node.end] > dp[curEnd] + node.fare) {
                    dp[node.end] = dp[curEnd] + node.fare;
                    pq.offer(new Node(node.end, dp[node.end]));
                }
            }
        }
    }

    public static class Node implements Comparable<Node> {
        int end, fare;

        public Node(final int end, final int fare) {
            this.end = end;
            this.fare = fare;
        }

        /**
         * 우선순위 큐의 판단 기준 : fare
         * 같은 경로이지만 비용이 다른 경우 fare가 더 적은 경로를 선택!
         */
        @Override
        public int compareTo(final Node o) {
            return fare - o.fare;
        }
    }
}
```