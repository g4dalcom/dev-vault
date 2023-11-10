# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/49189)

## 📝 문제

n개의 노드가 있는 그래프가 있습니다. 각 노드는 1부터 n까지 번호가 적혀있습니다. 1번 노드에서 가장 멀리 떨어진 노드의 갯수를 구하려고 합니다. 가장 멀리 떨어진 노드란 최단경로로 이동했을 때 간선의 개수가 가장 많은 노드들을 의미합니다.

노드의 개수 n, 간선에 대한 정보가 담긴 2차원 배열 vertex가 매개변수로 주어질 때, 1번 노드로부터 가장 멀리 떨어진 노드가 몇 개인지를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 노드의 개수 n은 2 이상 20,000 이하입니다.
- 간선은 양방향이며 총 1개 이상 50,000개 이하의 간선이 있습니다.
- vertex 배열 각 행 [a, b]는 a번 노드와 b번 노드 사이에 간선이 있다는 의미입니다.

##### 입출력 예

|n|vertex|return|
|---|---|---|
|6|[[3, 6], [4, 3], [3, 2], [1, 3], [1, 2], [2, 4], [5, 2]]|3|

##### 입출력 예 설명

예제의 그래프를 표현하면 아래 그림과 같고, 1번 노드에서 가장 멀리 떨어진 노드는 4,5,6번 노드입니다.

![image.png](https://grepp-programmers.s3.amazonaws.com/files/ybm/fadbae38bb/dec85ab5-0273-47b3-ba73-fc0b5f6be28a.png)

---

### 💡 풀이

- BFS를 이용해서 최단거리를 구하는 흔한 문제인데 여기서 최단거리가 가장 긴 노드의 개수를 추가적으로 구하는 것이었습니다.
- 연결된 노드들을 양방향 인접 리스트로 묶어준 후 BFS를 돌았고 한 번의 사이클이 지날 때마다 dp를 +1씩 해주며 최단거리를 구하였습니다.
- 그리고 최단거리를 구할 때마다 가장 긴 거리(maxEdge)를 갱신해주면서 가장 긴 노드의 거리를 구하고 최종적으로 maxEdge와 같은 dp값이 몇 개인지 세어주었습니다!

### 🔍 정답

```java
import java.util.*;

class Solution {
    static int[] dp;
    static boolean[] visit;
    static ArrayList<ArrayList<Integer>> nodeList;
    static int maxEdge = 0;
    
    public int solution(int n, int[][] edge) {
        dp = new int[n+1];
        visit = new boolean[n+1];
        nodeList = new ArrayList<>();
        
        for (int i = 0; i <= n; i++) {
            nodeList.add(new ArrayList<>());
        }
        
        for (int i = 0; i < edge.length; i++) {
            int a = edge[i][0];
            int b = edge[i][1];
            
            nodeList.get(a).add(b);
            nodeList.get(b).add(a);
        }
        
        bfs(1);
        
        int sum = 0;
        for (int i = 0; i < dp.length; i++) {
            if (dp[i] == maxEdge) sum++;
        }
        
        return sum;
    }
    
    public void bfs(int start) {
        Queue<Integer> q = new LinkedList<>();
        q.add(start);
        visit[start] = true;
        
        while (!q.isEmpty()) {
            int current = q.poll();
            
            for (int edge : nodeList.get(current)) {
                if (!visit[edge]) {
                    visit[edge] = true;
                    dp[edge] = dp[current] + 1;
                    q.add(edge);
                    maxEdge = Math.max(maxEdge, dp[edge]);
                }
            }
        }
    }
}
```