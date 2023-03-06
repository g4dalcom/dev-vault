# [문제링크](https://www.acmicpc.net/problem/1197)

## 📝 문제

그래프가 주어졌을 때, 그 그래프의 최소 스패닝 트리를 구하는 프로그램을 작성하시오.

최소 스패닝 트리는, 주어진 그래프의 모든 정점들을 연결하는 부분 그래프 중에서 그 가중치의 합이 최소인 트리를 말한다.

## 입력

첫째 줄에 정점의 개수 V(1 ≤ V ≤ 10,000)와 간선의 개수 E(1 ≤ E ≤ 100,000)가 주어진다. 다음 E개의 줄에는 각 간선에 대한 정보를 나타내는 세 정수 A, B, C가 주어진다. 이는 A번 정점과 B번 정점이 가중치 C인 간선으로 연결되어 있다는 의미이다. C는 음수일 수도 있으며, 절댓값이 1,000,000을 넘지 않는다.

그래프의 정점은 1번부터 V번까지 번호가 매겨져 있고, 임의의 두 정점 사이에 경로가 있다. 최소 스패닝 트리의 가중치가 -2,147,483,648보다 크거나 같고, 2,147,483,647보다 작거나 같은 데이터만 입력으로 주어진다.

## 출력

첫째 줄에 최소 스패닝 트리의 가중치를 출력한다.

## 예제 입력 1 

3 3
1 2 1
2 3 2
1 3 3

## 예제 출력 1 

3

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.PriorityQueue;
import java.util.StringTokenizer;

public class Main {
    static int V, E;
    static PriorityQueue<Node> pq;
    static int[] parent;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        V = Integer.parseInt(st.nextToken());   // 정점의 개수
        E = Integer.parseInt(st.nextToken());   // 간선의 개수

        pq = new PriorityQueue<>();     // 가중치를 기준으로 오름차순으로 정렬한 큐

        for (int i = 0; i < E; i++) {
            st = new StringTokenizer(br.readLine());
            int start = Integer.parseInt(st.nextToken())-1;
            int end = Integer.parseInt(st.nextToken())-1;
            int weight = Integer.parseInt(st.nextToken());

            pq.offer(new Node(start, end, weight));
        }

        // 유니온파인드에 앞서 부모 노드를 자기 자신으로 초기화!
        parent = new int[V];
        for (int i = 0; i < V; i++) {
            parent[i] = i;
        }

        int sum = 0;
        while (!pq.isEmpty()) {
            Node node = pq.poll();

            // 같은 부모가 아닐 경우(사이클을 형성하지 않을 경우) 이어주고 가중치를 더하기!
            if (!(isSameParent(node.start, node.end))) {
                union(node.start, node.end);
                sum += node.weight;
            }
        }
        System.out.println(sum);
    }

    public static int find(int x) {
        if (x == parent[x]) {
            return x;
        }
        return parent[x] = find(parent[x]);
    }

    public static void union(int a, int b) {
        a = find(a);
        b = find(b);

        if (a != b) {
            if (a > b)
                parent[b] = a;
            else
                parent[a] = b;
        }
    }
    public static boolean isSameParent(int a, int b) {
        a = find(a);
        b = find(b);

        if (a == b)
            return true;
        else
            return false;
    }

    public static class Node implements Comparable<Node> {
        int start, end, weight;

        public Node(final int start, final int end, final int weight) {
            this.start = start;
            this.end = end;
            this.weight = weight;
        }

        @Override
        public int compareTo(final Node o) {
            return weight - o.weight;
        }
    }
}
```
[[크루스칼_알고리즘]]