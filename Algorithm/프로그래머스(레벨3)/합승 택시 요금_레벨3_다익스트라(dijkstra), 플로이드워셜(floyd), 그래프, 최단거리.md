# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/72413)

## 📝 문제

밤늦게 귀가할 때 안전을 위해 항상 택시를 이용하던 `무지`는 최근 야근이 잦아져 택시를 더 많이 이용하게 되어 택시비를 아낄 수 있는 방법을 고민하고 있습니다. "무지"는 자신이 택시를 이용할 때 동료인 `어피치` 역시 자신과 비슷한 방향으로 가는 택시를 종종 이용하는 것을 알게 되었습니다. "무지"는 "어피치"와 귀가 방향이 비슷하여 택시 합승을 적절히 이용하면 택시요금을 얼마나 아낄 수 있을 지 계산해 보고 "어피치"에게 합승을 제안해 보려고 합니다.

![2021_kakao_taxi_01.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/715ff493-d1a0-44d8-9273-a785280b3f1e/2021_kakao_taxi_01.png)

위 예시 그림은 택시가 이동 가능한 반경에 있는 6개 지점 사이의 이동 가능한 택시노선과 예상요금을 보여주고 있습니다.  
그림에서 `A`와 `B` 두 사람은 출발지점인 4번 지점에서 출발해서 택시를 타고 귀가하려고 합니다. `A`의 집은 6번 지점에 있으며 `B`의 집은 2번 지점에 있고 두 사람이 모두 귀가하는 데 소요되는 예상 최저 택시요금이 얼마인 지 계산하려고 합니다.

- 그림의 원은 지점을 나타내며 원 안의 숫자는 지점 번호를 나타냅니다.
    - 지점이 n개일 때, 지점 번호는 1부터 n까지 사용됩니다.
- 지점 간에 택시가 이동할 수 있는 경로를 간선이라 하며, 간선에 표시된 숫자는 두 지점 사이의 예상 택시요금을 나타냅니다.
    - 간선은 편의 상 직선으로 표시되어 있습니다.
    - 위 그림 예시에서, 4번 지점에서 1번 지점으로(4→1) 가거나, 1번 지점에서 4번 지점으로(1→4) 갈 때 예상 택시요금은 `10`원으로 동일하며 이동 방향에 따라 달라지지 않습니다.
- 예상되는 최저 택시요금은 다음과 같이 계산됩니다.
    - 4→1→5 : `A`, `B`가 합승하여 택시를 이용합니다. 예상 택시요금은 `10 + 24 = 34`원 입니다.
    - 5→6 : `A`가 혼자 택시를 이용합니다. 예상 택시요금은 `2`원 입니다.
    - 5→3→2 : `B`가 혼자 택시를 이용합니다. 예상 택시요금은 `24 + 22 = 46`원 입니다.
    - `A`, `B` 모두 귀가 완료까지 예상되는 최저 택시요금은 `34 + 2 + 46 = 82`원 입니다.

---

#### **[문제]**

지점의 개수 n, 출발지점을 나타내는 s, `A`의 도착지점을 나타내는 a, `B`의 도착지점을 나타내는 b, 지점 사이의 예상 택시요금을 나타내는 fares가 매개변수로 주어집니다. 이때, `A`, `B` 두 사람이 s에서 출발해서 각각의 도착 지점까지 택시를 타고 간다고 가정할 때, 최저 예상 택시요금을 계산해서 return 하도록 solution 함수를 완성해 주세요.  
만약, 아예 합승을 하지 않고 각자 이동하는 경우의 예상 택시요금이 더 낮다면, 합승을 하지 않아도 됩니다.

#### **[제한사항]**

- 지점갯수 n은 3 이상 200 이하인 자연수입니다.
- 지점 s, a, b는 1 이상 n 이하인 자연수이며, 각기 서로 다른 값입니다.
    - 즉, 출발지점, `A`의 도착지점, `B`의 도착지점은 서로 겹치지 않습니다.
- fares는 2차원 정수 배열입니다.
- fares 배열의 크기는 2 이상 `n x (n-1) / 2` 이하입니다.
    - 예를들어, n = 6이라면 fares 배열의 크기는 2 이상 15 이하입니다. (`6 x 5 / 2 = 15`)
    - fares 배열의 각 행은 [c, d, f] 형태입니다.
    - c지점과 d지점 사이의 예상 택시요금이 `f`원이라는 뜻입니다.
    - 지점 c, d는 1 이상 n 이하인 자연수이며, 각기 서로 다른 값입니다.
    - 요금 f는 1 이상 100,000 이하인 자연수입니다.
    - fares 배열에 두 지점 간 예상 택시요금은 1개만 주어집니다. 즉, [c, d, f]가 있다면 [d, c, f]는 주어지지 않습니다.
- 출발지점 s에서 도착지점 a와 b로 가는 경로가 존재하는 경우만 입력으로 주어집니다.

---

##### **[입출력 예]**

|n|s|a|b|fares|result|
|---|---|---|---|---|---|
|6|4|6|2|[[4, 1, 10], [3, 5, 24], [5, 6, 2], [3, 1, 41], [5, 1, 24], [4, 6, 50], [2, 4, 66], [2, 3, 22], [1, 6, 25]]|82|
|7|3|4|1|[[5, 7, 9], [4, 6, 4], [3, 6, 1], [3, 2, 3], [2, 1, 6]]|14|
|6|4|5|6|[[2,6,6], [6,3,7], [4,6,7], [6,5,11], [2,5,12], [5,3,20], [2,4,8], [4,3,9]]|18|

##### **입출력 예에 대한 설명**

---

**입출력 예 #1**  
문제 예시와 같습니다.

**입출력 예 #2**  
![2021_kakao_taxi_02.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/934fcb5a-f844-4b02-b7fa-46198123be05/2021_kakao_taxi_02.png)

- 합승을 하지 않고, `B`는 `3→2→1`, `A`는 `3→6→4` 경로로 각자 택시를 타고 가는 것이 최저 예상 택시요금입니다.
- 따라서 최저 예상 택시요금은 `(3 + 6) + (1 + 4) = 14`원 입니다.

**입출력 예 #3**  
![2021_kakao_taxi_03.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/179cc8ad-73d2-46c9-95e9-2363f3cb345d/2021_kakao_taxi_03.png)

- `A`와 `B`가 `4→6` 구간을 합승하고 `B`가 6번 지점에서 내린 후, `A가`6→5` 구간을 혼자 타고 가는 것이 최저 예상 택시요금입니다.
- 따라서 최저 예상 택시요금은 `7 + 11 = 18`원 입니다.

---

### 💡 풀이

- 출발 지점(S) 에서 무지의 목적지(A)와 어피치의 목적지(B)로 각각 도달하는 최소 비용을 계산해야 합니다.
- 노드 사이에 모두 다른 비용(가중치)이 정의되어 있기 때문에 단순 BFS가 아니라 우선순위큐로 가중치를 기준으로 오름차순 정렬한 뒤 연산을 해야 합니다.
- 단순히 S에서 A와 B로 가는 최소비용을 구해야 한다면 한 번의 다익스트라 연산을 한 뒤 dp[A] + dp[B] 를 하면 되겠지만 이 문제에서는 **합승** 을 통해서 더 적은 비용으로 목적지에 도달할 수 있는지 여부도 확인을 해야 합니다.
- 쉽게 생각하면 S를 기준으로 다익스트라를 구현해서 모든 노드로의 최소 비용을 구한 뒤 dp[A] + dp[B] 를 해주고,
- S에서 갈 수 있는 노드로 이동한 뒤 해당 노드를 기준으로 다익스트라를 구현해서 **S에서 이동한 비용(합승) + dp[A] + dp[B]** 를 하는 식으로 여러 번의 다익스트라 연산을 통해서 구할 수 있겠지만 이 방법은 효율성 테스트를 통과하지 못합니다!
- 따라서 최소한의 다익스트라 연산을 해야 하는데 어떠한 조건에서도 세 번의 다익스트라 연산으로도 모든 경우를 탐색할 수 있습니다.

```java
int[] s_base = dijkstra(s);
int[] a_base = dijkstra(a);
int[] b_base = dijkstra(b);

for (int i = 1; i <= n; i++) {
	min_fare = Math.min(min_fare, s_base[i] + a_base[i] + b_base[i]);
}
```

- 양방향 그래프로 구현이 되므로 A -> B 는 B -> A 와 같습니다. x라는 노드에서 a로 이동하나 a에서 x라는 노드로 이동하는 것은 같다는 의미입니다.
- 따라서 S를 기준으로 **x** 까지의 가중치 + A를 기준으로 **x** 까지의 가중치 + B를 기준으로 **x** 까지의 가중치를 구한다면 S에서 x로 이동한 뒤 x에서 A, B로 각각 이동한 것과 같은 연산을 할 수 있는 거겠죠?!
- 그러면 우리는 각각의 노드를 기준으로 다익스트라를 구현할 필요 없이, S, A, B를 기준으로만 연산을 한다면 모든 경우를 구할 수 있는 것입니다!

### 🔍 정답 (다익스트라)

```java
import java.util.*;

class Solution {
    static int min_fare = Integer.MAX_VALUE;
    static ArrayList<ArrayList<Node>> nodes;
    static int n;
    
    public int solution(int n, int s, int a, int b, int[][] fares) {
        this.n = n;
        nodes = new ArrayList<>();
        for (int i = 0; i <= n; i++) {
            nodes.add(new ArrayList<>());
        }
        
        for (int i = 0; i < fares.length; i++) {
            int from = fares[i][0];
            int to = fares[i][1];
            int fare = fares[i][2];
            
            nodes.get(from).add(new Node(to, fare));
            nodes.get(to).add(new Node(from, fare));
        }
        
        int[] s_base = dijkstra(s);
        int[] a_base = dijkstra(a);
        int[] b_base = dijkstra(b);
        
        for (int i = 1; i <= n; i++) {
            min_fare = Math.min(min_fare, s_base[i] + a_base[i] + b_base[i]);
        }
        
        return min_fare;
    }
    
    public int[] dijkstra(int start) {
        PriorityQueue<Node> pq = new PriorityQueue<>();
        int[] dp = new int[n+1];
        Arrays.fill(dp, Integer.MAX_VALUE);
        boolean[] visit = new boolean[n+1];
        
        dp[start] = 0;
        pq.add(new Node(start, 0));
        
        while (!pq.isEmpty()) {
            Node current = pq.poll();
            int to = current.destination;
            
            if (!visit[to]) {
                visit[to] = true;
                
                for (Node next : nodes.get(to)) {
                    if (dp[next.destination] > dp[to] + next.fare) {
                        dp[next.destination] = dp[to] + next.fare;
                        pq.add(new Node(next.destination, dp[next.destination]));
                    }
                }
            }
        }
        
        return dp;
    }
    
    public class Node implements Comparable<Node> {
        int destination, fare;
        
        public Node(int destination, int fare) {
            this.destination = destination;
            this.fare = fare;
        }
        
        @Override
        public int compareTo(Node n) {
            return this.fare - n.fare;
        }
    }
}
```


### 🔍 정답 (플로이드 워셜)

```java
class Solution {
    public int solution(int n, int s, int a, int b, int[][] fares) {
        final int MAX_FARE = 100000 * n + 1;
        int[][] floyd = new int[n+1][n+1];
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if (i != j) {
                    floyd[i][j] = MAX_FARE;
                }
            }
        }
        
        for (int i = 0; i < fares.length; i++) {
            int from = fares[i][0];
            int to = fares[i][1];
            int fare = fares[i][2];
            
            floyd[from][to] = fare;
            floyd[to][from] = fare;
        }
         
        for (int k = 1; k <= n; k++) {
            for (int i = 1; i <= n; i++) {
                for (int j = 1; j <= n; j++) {
                    if (floyd[i][j] > floyd[i][k] + floyd[k][j]) {
                        floyd[i][j] = floyd[i][k] + floyd[k][j];
                    }
                }
            }
        }
        
        int min_fare = floyd[s][a] + floyd[s][b];
        for (int i = 1; i <= n; i++) {
            min_fare = Math.min(min_fare, floyd[s][i] + floyd[a][i] + floyd[b][i]);
        }
        
        return min_fare;
    }
}
```

- 플로이드 워셜은 모든 노드에서 다른 모든 노드로의 최소 비용을 구하는 알고리즘입니다.
- 기본적으로는 다익스트라와 같은 접근 방식을 사용합니다.
- 다익스트라로는 세 번의 연산으로 구할 수 있지만 플로이드 워셜은 모든 경우를 연산하기 위해 3중 반복문을 사용한다는 데에서 성능은 떨어질 수 있지만 이 문제의 효율성 테스트를 통과하는 데는 문제가 없었습니다.