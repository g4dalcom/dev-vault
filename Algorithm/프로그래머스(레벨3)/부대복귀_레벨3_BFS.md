# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/132266)

## 📝 문제

강철부대의 각 부대원이 여러 지역에 뿔뿔이 흩어져 특수 임무를 수행 중입니다. 지도에서 강철부대가 위치한 지역을 포함한 각 지역은 유일한 번호로 구분되며, 두 지역 간의 길을 통과하는 데 걸리는 시간은 모두 1로 동일합니다. 임무를 수행한 각 부대원은 지도 정보를 이용하여 최단시간에 부대로 복귀하고자 합니다. 다만 적군의 방해로 인해, 임무의 시작 때와 다르게 되돌아오는 경로가 없어져 복귀가 불가능한 부대원도 있을 수 있습니다.

강철부대가 위치한 지역을 포함한 총지역의 수 `n`, 두 지역을 왕복할 수 있는 길 정보를 담은 2차원 정수 배열 `roads`, 각 부대원이 위치한 서로 다른 지역들을 나타내는 정수 배열 `sources`, 강철부대의 지역 `destination`이 주어졌을 때, 주어진 `sources`의 원소 순서대로 강철부대로 복귀할 수 있는 최단시간을 담은 배열을 return하는 solution 함수를 완성해주세요. 복귀가 불가능한 경우 해당 부대원의 최단시간은 -1입니다.

##### 제한사항

- 3 ≤ `n` ≤ 100,000
    - 각 지역은 정수 1부터 `n`까지의 번호로 구분됩니다.
- 2 ≤ `roads`의 길이 ≤ 500,000
    - `roads`의 원소의 길이 = 2
    - `roads`의 원소는 [a, b] 형태로 두 지역 a, b가 서로 왕복할 수 있음을 의미합니다.(1 ≤ a, b ≤ n, a ≠ b)
    - 동일한 정보가 중복해서 주어지지 않습니다.
        - 동일한 [a, b]가 중복해서 주어지지 않습니다.
        - [a, b]가 있다면 [b, a]는 주어지지 않습니다.
- 1 ≤ `sources`의 길이 ≤ 500
    - 1 ≤ `sources[i]` ≤ n
- 1 ≤ `destination` ≤ n

---

##### 입출력 예

|n|roads|sources|destination|result|
|---|---|---|---|---|
|3|[[1, 2], [2, 3]]|[2, 3]|1|[1, 2]|
|5|[[1, 2], [1, 4], [2, 4], [2, 5], [4, 5]]|[1, 3, 5]|5|[2, -1, 0]|

---

##### 입출력 예 설명

**입출력 예 #1**

- 지역 2는 지역 1과 길로 연결되어 있기 때문에, 지역 2에서 지역 1의 최단거리는 1입니다.
- 지역 3에서 지역 1로 이동할 수 있는 최단경로는 지역 3 → 지역 2 → 지역 1 순으로 이동하는 것이기 때문에, 지역 3에서 지역 1의 최단거리는 2입니다.
- 따라서 [1, 2]를 return합니다.

**입출력 예 #2**

- 지역 1에서 지역 5의 최단경로는 지역 1 → 지역 2 → 지역 5 또는 지역 1 → 지역 4 → 지역 5 순으로 이동하는 것이기 때문에, 최단거리는 2입니다.
- 지역 3에서 지역 5로 가는 경로가 없기 때문에, 지역 3에서 지역 5로 가는 최단거리는 -1입니다.
- 지역 5에서 지역 5는 이동할 필요가 없기 때문에, 최단거리는 0입니다.
- 따라서 [2, -1, 0]을 return합니다.

---

### 💡 풀이

- 인접한 구역은 양방향 리스트로 저장해줍니다.

```java
for (int i = 0; i <= n; i++) {
	areaInfo.add(new ArrayList<>());
}

for (int i = 0; i < roads.length; i++) {
	int a = roads[i][0];
	int b = roads[i][1];

	areaInfo.get(a).add(b);
	areaInfo.get(b).add(a);
}
```

- 그리고 최단거리를 구하는 문제이기 때문에 BFS 탐색을 해주는데 처음에는 sources에 있는 구역을 꺼내서 BFS 탐색을 순차적으로 해주면 10번 이후의 테스트케이스부터는 시간 초과가 발생합니다.
- sources 배열이 500개까지이므로 500번 큐를 생성하고 계속적으로 탐색하는 작업이 무거운 것이겠죠!
- 이런 경우 목적지에서부터 반대로 탐색을 하게되면 단 한 번의 탐색으로 모든 구역의 최단거리를 구할 수 있습니다.
- 만약 sources가 1, 3, 5이고 목적지가 7이라면 1번에서 7번까지의 경로를 탐색하고 3번에서 7번까지의 경로를 탐색하고 5번에서 7번까지의 경로를 탐색할텐데 이 때 중복 탐색하는 경우가 많이 발생하겠죠!
- 그러나 7번에서 탐색을 시작하면 가장 가까운 곳은 1, 한 번 거치는 곳은 2, 그 이후는 3, 4, 5 이런 식으로 BFS탐색 한 번으로 모든 구역을 탐색할 수 있게 됩니다!

### 🔍 정답

```java
import java.util.*;

class Solution {
    static ArrayList<ArrayList<Integer>> areaInfo = new ArrayList<>();
    static int[] combackTime;
    static int[] areaTime;

    public int[] solution(int n, int[][] roads, int[] sources, int destination) {  
        combackTime = new int[sources.length];
        areaTime = new int[n+1];
        Arrays.fill(areaTime, -1);

        for (int i = 0; i <= n; i++) {
            areaInfo.add(new ArrayList<>());
        }

        for (int i = 0; i < roads.length; i++) {
            int a = roads[i][0];
            int b = roads[i][1];

            areaInfo.get(a).add(b);
            areaInfo.get(b).add(a);
        }

        bfs(destination);

        for (int i = 0; i < sources.length; i++) {
            combackTime[i] = areaTime[sources[i]];
        }

        return combackTime;
    }

    public void bfs(int destination) {
        Queue<Node> q = new LinkedList<>();
        q.add(new Node(destination, 0));
        areaTime[destination] = 0;

        while (!q.isEmpty()) {
            Node current = q.poll();

            for (int node : areaInfo.get(current.area)) {
                if (areaTime[node] == -1) {
                    q.add(new Node(node, current.time+1));
                    areaTime[node] = current.time+1;
                }
            }
        }  
    }

    public class Node {
        int area, time;

        public Node(int area, int time) {
            this.area = area;
            this.time = time;
        }
    }
}
```