# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12978?language=java)

## 📝 문제

N개의 마을로 이루어진 나라가 있습니다. 이 나라의 각 마을에는 1부터 N까지의 번호가 각각 하나씩 부여되어 있습니다. 각 마을은 양방향으로 통행할 수 있는 도로로 연결되어 있는데, 서로 다른 마을 간에 이동할 때는 이 도로를 지나야 합니다. 도로를 지날 때 걸리는 시간은 도로별로 다릅니다. 현재 1번 마을에 있는 음식점에서 각 마을로 음식 배달을 하려고 합니다. 각 마을로부터 음식 주문을 받으려고 하는데, N개의 마을 중에서 K 시간 이하로 배달이 가능한 마을에서만 주문을 받으려고 합니다. 다음은 N = 5, K = 3인 경우의 예시입니다.

![배달_1_uxun8t.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d7779d88-084c-4ffa-ae9f-2a42f97d3bbf/%E1%84%87%E1%85%A2%E1%84%83%E1%85%A1%E1%86%AF_1_uxun8t.png)

위 그림에서 1번 마을에 있는 음식점은 \[1, 2, 4, 5\] 번 마을까지는 3 이하의 시간에 배달할 수 있습니다. 그러나 3번 마을까지는 3시간 이내로 배달할 수 있는 경로가 없으므로 3번 마을에서는 주문을 받지 않습니다. 따라서 1번 마을에 있는 음식점이 배달 주문을 받을 수 있는 마을은 4개가 됩니다.  
마을의 개수 N, 각 마을을 연결하는 도로의 정보 road, 음식 배달이 가능한 시간 K가 매개변수로 주어질 때, 음식 주문을 받을 수 있는 마을의 개수를 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

- 마을의 개수 N은 1 이상 50 이하의 자연수입니다.
- road의 길이(도로 정보의 개수)는 1 이상 2,000 이하입니다.
- road의 각 원소는 마을을 연결하고 있는 각 도로의 정보를 나타냅니다.
- road는 길이가 3인 배열이며, 순서대로 (a, b, c)를 나타냅니다.
    - a, b(1 ≤ a, b ≤ N, a != b)는 도로가 연결하는 두 마을의 번호이며, c(1 ≤ c ≤ 10,000, c는 자연수)는 도로를 지나는데 걸리는 시간입니다.
    - 두 마을 a, b를 연결하는 도로는 여러 개가 있을 수 있습니다.
    - 한 도로의 정보가 여러 번 중복해서 주어지지 않습니다.
- K는 음식 배달이 가능한 시간을 나타내며, 1 이상 500,000 이하입니다.
- 임의의 두 마을간에 항상 이동 가능한 경로가 존재합니다.
- 1번 마을에 있는 음식점이 K 이하의 시간에 배달이 가능한 마을의 개수를 return 하면 됩니다.

---

##### 입출력 예

|N|road|K|result|
|---|---|---|---|
|5|[[1,2,1],[2,3,3],[5,2,2],[1,4,2],[5,3,1],[5,4,2]]|3|4|
|6|[[1,2,1],[1,3,2],[2,3,2],[3,4,3],[3,5,2],[3,5,3],[5,6,1]]|4|4|

##### 입출력 예 설명

입출력 예 #1  
문제의 예시와 같습니다.

입출력 예 #2  
주어진 마을과 도로의 모양은 아래 그림과 같습니다.  
![배달_3_njc7kq.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/993685f2-6b97-4fe3-85b5-47c085dc1bf3/%E1%84%87%E1%85%A2%E1%84%83%E1%85%A1%E1%86%AF_3_njc7kq.png)  
1번 마을에서 배달에 4시간 이하가 걸리는 마을은 [1, 2, 3, 5] 4개이므로 4를 return 합니다.

---

### 💡 풀이

- 1번 마을이라는 하나의 정점에서 다른 정점으로의 거리를 계산해야 하므로 다익스트라 알고리즘을 사용하면 된다!
- [다익스트라 알고리즘 정리](https://github.com/g4dalcom/dev_vault/blob/main/Algorithm/%EB%8B%A4%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%9D%BC(dijkstra)_%ED%94%8C%EB%A1%9C%EC%9D%B4%EB%93%9C%EC%9B%8C%EC%85%9C_%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98.md)

### 🔍 정답

```java
import java.util.*;

class Solution {
    
    static int[] dp;
    static boolean[] check;
    static ArrayList<ArrayList<Node>> list;
    
    public int solution(int N, int[][] road, int K) {
        dp = new int[N+1];
        check = new boolean[N+1];
        
        list = new ArrayList<>();
        for (int i = 0; i < N+1; i++) {
            list.add(new ArrayList<>());
        }
        
        for (int i = 0; i < road.length; i++) {
            int a = road[i][0];
            int b = road[i][1];
            int weight = road[i][2];
            
            list.get(a).add(new Node(b, weight));
            list.get(b).add(new Node(a, weight));
        }
        
        dijkstra(1);
        
        int count = 0;
        for (int v : dp) {
            if (v <= K) count++;
        }
        
        return count;
    }
    
    public void dijkstra(int start) {
        Queue<Node> q = new PriorityQueue<>();
        q.offer(new Node(start, 0));
        
        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[start] = 0;
        
        while (!q.isEmpty()) {
            Node node = q.poll();
            int to = node.to;
            
            if (!check[to]) {
                check[to] = true;
                
                for (Node next : list.get(to)) {
                    if (dp[next.to] > dp[to] + next.weight) {
                        dp[next.to] = dp[to] + next.weight;
                        q.offer(new Node(next.to, dp[next.to]));
                    }
                }
            }
        }
    }
    
    public class Node implements Comparable<Node> {
        int to, weight;
        
        public Node(int to, int weight) {
            this.to = to;
            this.weight = weight;
        }
        
        @Override
        public int compareTo(Node o) {
            return weight - o.weight;
        }
    }
}
```