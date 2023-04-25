# [문제링크](https://www.acmicpc.net/problem/1504)

## 📝 문제

방향성이 없는 그래프가 주어진다. 세준이는 1번 정점에서 N번 정점으로 최단 거리로 이동하려고 한다. 또한 세준이는 두 가지 조건을 만족하면서 이동하는 특정한 최단 경로를 구하고 싶은데, 그것은 바로 임의로 주어진 두 정점은 반드시 통과해야 한다는 것이다.

세준이는 한번 이동했던 정점은 물론, 한번 이동했던 간선도 다시 이동할 수 있다. 하지만 반드시 최단 경로로 이동해야 한다는 사실에 주의하라. 1번 정점에서 N번 정점으로 이동할 때, 주어진 두 정점을 반드시 거치면서 최단 경로로 이동하는 프로그램을 작성하시오.

## 입력

첫째 줄에 정점의 개수 N과 간선의 개수 E가 주어진다. (2 ≤ N ≤ 800, 0 ≤ E ≤ 200,000) 둘째 줄부터 E개의 줄에 걸쳐서 세 개의 정수 a, b, c가 주어지는데, a번 정점에서 b번 정점까지 양방향 길이 존재하며, 그 거리가 c라는 뜻이다. (1 ≤ c ≤ 1,000) 다음 줄에는 반드시 거쳐야 하는 두 개의 서로 다른 정점 번호 v1과 v2가 주어진다. (v1 ≠ v2, v1 ≠ N, v2 ≠ 1) 임의의 두 정점 u와 v사이에는 간선이 최대 1개 존재한다.

## 출력

첫째 줄에 두 개의 정점을 지나는 최단 경로의 길이를 출력한다. 그러한 경로가 없을 때에는 -1을 출력한다.

## 예제 입력 1 

4 6
1 2 3
2 3 3
3 4 1
1 3 5
2 4 5
1 4 4
2 3

## 예제 출력 1 

7

---

### 🔍 정답

```java
import java.util.*;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    static int N, M;
    static ArrayList<ArrayList<Node>> map = new ArrayList<>();
    static int[] dp;
    static boolean[] visit;
    static final int INF = 1000 * 200000;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());   // 정점의 개수
        M = Integer.parseInt(st.nextToken());   // 간선의 개수

        for (int i = 0; i <= N; i++) {
            map.add(new ArrayList<>());
        }

        // 방향성이 없기 때문에 양방향 인접 리스트를 구성
        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());
            int start = Integer.parseInt(st.nextToken());
            int end = Integer.parseInt(st.nextToken());
            int weight = Integer.parseInt(st.nextToken());

            map.get(start).add(new Node(end, weight));
            map.get(end).add(new Node(start, weight));
        }

        st = new StringTokenizer(br.readLine());
        int E1 = Integer.parseInt(st.nextToken());
        int E2 = Integer.parseInt(st.nextToken());

        dp = new int[N+1];
        visit = new boolean[N+1];

        /**
         * 경로 1 : 1 -> E1 -> E2 -> N, 경로 2 : 1 -> E2 -> E1 -> N
         * 두 경로 중 더 비용이 적은 쪽을 선택한다!
         */
        int sum1 = 0;
        sum1 += dijkstra(1, E1);
        sum1 += dijkstra(E1, E2);
        sum1 += dijkstra(E2, N);

        int sum2 = 0;
        sum2 += dijkstra(1, E2);
        sum2 += dijkstra(E2, E1);
        sum2 += dijkstra(E1, N);

        System.out.println(sum1 >= INF && sum2 >= INF ? -1 : Math.min(sum1, sum2));

    }

    public static int dijkstra(int start, int end) {
        PriorityQueue<Node> pq = new PriorityQueue<>();
        Arrays.fill(dp, INF);
        Arrays.fill(visit, false);

        // start에서 start까지의 거리는 0이므로 초깃값 0으로 설정!
        dp[start] = 0;
        pq.offer(new Node(start, 0));

        while (!pq.isEmpty()) {
            Node node = pq.poll();
            int cur = node.end;

            if (!visit[cur]) {
                visit[cur] = true;

                // 기존에 저장된 비용보다, 현재까지의 비용 + 이동할 시 비용이 더 적다면 갱신!
                for (Node n : map.get(cur)) {
                    if (!visit[n.end] && dp[n.end] > dp[cur] + n.weight) {
                        dp[n.end] = dp[cur] + n.weight;
                        pq.offer(new Node(n.end, dp[n.end]));
                    }
                }
            }
        }
        return dp[end];


    }

    public static class Node implements Comparable<Node> {
        int end, weight;

        public Node (final int end, final int weight) {
            this.end = end;
            this.weight = weight;
        }

        // 우선순위 큐를 거리를 기준으로 오름차순 정렬
        public int compareTo(Node o) {
            return this.weight - o.weight;
        }
    }
}
```