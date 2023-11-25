# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42861)

## 📝 문제

n개의 섬 사이에 다리를 건설하는 비용(costs)이 주어질 때, 최소의 비용으로 모든 섬이 서로 통행 가능하도록 만들 때 필요한 최소 비용을 return 하도록 solution을 완성하세요.

다리를 여러 번 건너더라도, 도달할 수만 있으면 통행 가능하다고 봅니다. 예를 들어 A 섬과 B 섬 사이에 다리가 있고, B 섬과 C 섬 사이에 다리가 있으면 A 섬과 C 섬은 서로 통행 가능합니다.

**제한사항**

- 섬의 개수 n은 1 이상 100 이하입니다.
- costs의 길이는 `((n-1) * n) / 2`이하입니다.
- 임의의 i에 대해, costs[i][0] 와 costs[i] [1]에는 다리가 연결되는 두 섬의 번호가 들어있고, costs[i] [2]에는 이 두 섬을 연결하는 다리를 건설할 때 드는 비용입니다.
- 같은 연결은 두 번 주어지지 않습니다. 또한 순서가 바뀌더라도 같은 연결로 봅니다. 즉 0과 1 사이를 연결하는 비용이 주어졌을 때, 1과 0의 비용이 주어지지 않습니다.
- 모든 섬 사이의 다리 건설 비용이 주어지지 않습니다. 이 경우, 두 섬 사이의 건설이 불가능한 것으로 봅니다.
- 연결할 수 없는 섬은 주어지지 않습니다.

**입출력 예**

|n|costs|return|
|---|---|---|
|4|[[0,1,1],[0,2,2],[1,2,5],[1,3,1],[2,3,8]]|4|

**입출력 예 설명**

costs를 그림으로 표현하면 다음과 같으며, 이때 초록색 경로로 연결하는 것이 가장 적은 비용으로 모두를 통행할 수 있도록 만드는 방법입니다.

![image.png](https://grepp-programmers.s3.amazonaws.com/files/production/13e2952057/f2746a8c-527c-4451-9a73-42129911fe17.png)

---

### 💡 풀이

- 예제를 보고 사이클을 형성하지 않으면서 최소 비용으로 연결한 최소신장트리를 떠올렸으나 크루스칼 알고리즘은 접한 지 오래되어서 익숙한 다익스트라로 먼저 구현해보았습니다.
- 0번을 기준으로 모든 노드를 최소비용으로 잇는다면 위와 같은 그래프를 그릴 수 있을 거라고 생각했기 때문입니다.

### ❌ 오답 (다익스트라)

```java
import java.util.*;

class Solution {
    static ArrayList<ArrayList<Node>> list = new ArrayList<>();
    static int[] dp;
    static int[] min_fare;
    static boolean[] visit;
    
    public int solution(int n, int[][] costs) {
        dp = new int[n];
        min_fare = new int[n];
        visit = new boolean[n];
        Arrays.fill(dp, Integer.MAX_VALUE);
        
        for (int i = 0; i < n; i++) {
            list.add(new ArrayList<>());
        }
        
        for (int i = 0; i < costs.length; i++) {
            int a = costs[i][0];
            int b = costs[i][1];
            int fare = costs[i][2];
            
            list.get(a).add(new Node(b, fare));
            list.get(b).add(new Node(a, fare));
        }
        
        dijkstra(0);
        
        int sum = 0;
        for (int i = 0; i < n; i++) {
            sum += min_fare[i];
        }
        
        return sum;
    }
    
    public void dijkstra(int start) {
        Queue<Node> q = new PriorityQueue<>((o1, o2) -> o1.fare - o2.fare);
        q.add(new Node(start, 0));
        dp[start] = 0;
        min_fare[start] = 0;
        
        while (!q.isEmpty()) {
            Node cur = q.poll();
            
            if (!visit[cur.id]) {
                visit[cur.id] = true;

                for (Node next : list.get(cur.id)) {
                    if (dp[next.id] > dp[cur.id] + next.fare) {
                        dp[next.id] = dp[cur.id] + next.fare;
                        min_fare[next.id] = next.fare;
                        q.add(new Node(next.id, dp[next.id]));
                    }
                }
            }
        }
    }
    
    public class Node {
        int id;
        int fare;
        
        public Node(int id, int fare) {
            this.id = id;
            this.fare = fare;
        }
    }
}
```

- 기본적인 다익스트라 구현방법으로는 dp 배열에 누적값이 담깁니다. 예를 들어서 0에서 3으로 가는 길은 1을 거쳐서 3으로 가는 것이므로 0 -> 1의 비용과 1 -> 3으로 가는 비용을 합친 2가 들어가는 것이죠.
- 그러나 우리는 각각의 코스마다의 비용을 알아야 하므로 min_fare라는 배열을 하나 더 선언해서 그 배열에 누적값이 아닌 fare만을 담아주었습니다.
- 이렇게 제출했을 때 2개의 테스트 케이스를 제외하고는 모두 실패가 나왔는데, 그 이유는 아마도 0번을 기준으로만 구했고 다른 노드를 기준으로 한다면 결과가 달라질 수 있었기 때문일 것입니다.
- 다익스트라를 노드의 개수만큼 반복해서 최소비용을 구할 수도 있겠으나 너무 비효율적이라고 생각해서 크루스칼 알고리즘과 프림 알고리즘으로 다시 풀어보았습니다.

### 🔍 정답(크루스칼)

```java
import java.util.*;

class Solution {
    static PriorityQueue<Node> pq;
    static int[] parent;

    public int solution(int n, int[][] costs) {
        pq = new PriorityQueue<>((o1, o2) -> o1.fare - o2.fare);
        for (int i = 0; i < costs.length; i++) {
            int a = costs[i][0];
            int b = costs[i][1];
            int fare = costs[i][2];

            pq.add(new Node(a, b, fare));
        }

        parent = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }

        int min_fare = 0;
        while (!pq.isEmpty()) {
            Node node = pq.poll();

            if (!isSameParent(node.start, node.end)) {
                union(node.start, node.end);
                min_fare += node.fare;
            }
        }

        return min_fare;
    }

    public boolean isSameParent(int a, int b) {
        a = find(a);
        b = find(b);

        return a == b;
    }

    public int find(int x) {
        if (parent[x] == x) return x;

        return find(parent[x]);
    }

    public void union(int a, int b) {
        a = find(a);
        b = find(b);

        if (a != b) {
            if (a < b) 
                parent[b] = a;
             else 
                parent[a] = b;
        }
    }

    public class Node {
        int start;
        int end;
        int fare;

        public Node(int start, int end, int fare) {
            this.start = start;
            this.end = end;
            this.fare = fare;
        }
    }
}
```

- 기본적으로 크루스칼 알고리즘의 흐름은 아래와 같습니다.
	- 가중치를 기준으로 오름차순 정렬을 한다. (같은 코스이나 가중치가 다를 때, 더 적은 가중치로 먼저 연결하기 위함)
	- 두 섬이 연결되지 않았다면(유니온 파인드) 이어주고 가중치를 누적한다.
- 가중치를 기준으로 정렬해야 하므로 우선순위큐를 사용해서 낮은 가중치 순서로 하나씩 꺼내서 탐색했고
- 두 섬의 연결 여부는 유니온파인드 알고리즘을 이용해서 같은 부모를 가지고 있는지 판단하였습니다.

### 🔍 정답(프림)

```java
import java.util.*;

class Solution {
    static ArrayList<ArrayList<Node>> list;
    static boolean[] visit;
    
    public int solution(int n, int[][] costs) {  
        list = new ArrayList<>();
        visit = new boolean[n];
        
        for (int i = 0; i < n; i++) {
            list.add(new ArrayList<>());
        }
        
        for (int i = 0; i < costs.length; i++) {
            int a = costs[i][0];
            int b = costs[i][1];
            int fare = costs[i][2];
            
            list.get(a).add(new Node(b, fare));
            list.get(b).add(new Node(a, fare));
        }

        return prim();
    }
    
    public int prim() {
        PriorityQueue<Node> pq = new PriorityQueue<>((o1, o2) -> o1.fare - o2.fare);
        pq.add(new Node(0, 0));
        
        int min_fare = 0;
        while (!pq.isEmpty()) {
            Node cur = pq.poll();
            
            if (!visit[cur.node]) {
                visit[cur.node] = true;
                min_fare += cur.fare;
                
                for (Node next : list.get(cur.node)) {
                    if (!visit[next.node]) {
                        pq.add(new Node(next.node, next.fare));
                    }
                }
            }
        }
        
        return min_fare;
    }
    
    public class Node {
        int node;
        int fare;
        
        public Node(int node, int fare) {
            this.node = node;
            this.fare = fare;
        }
    }
}
```

- 가중치를 최소힙으로 구현한 우선순위큐를 이용한 프림 알고리즘 풀이는,
- 인접 리스트로 연결된 노드들을 리스트에 담아주고 0번 노드부터 시작해서 꺼낸 노드와 방문하지 않은 연결된 노드를 모두 우선순위큐에 담아주고 가장 가중치가 적은 노드를 꺼내서 해당 노드의 가중치를 누적하는 방식으로 구현하였습니다.