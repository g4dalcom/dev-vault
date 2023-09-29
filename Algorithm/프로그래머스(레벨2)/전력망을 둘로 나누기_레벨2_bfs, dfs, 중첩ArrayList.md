# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/86971)

## 📝 문제

n개의 송전탑이 전선을 통해 하나의 [트리](https://en.wikipedia.org/wiki/Tree_(data_structure)) 형태로 연결되어 있습니다. 당신은 이 전선들 중 하나를 끊어서 현재의 전력망 네트워크를 2개로 분할하려고 합니다. 이때, 두 전력망이 갖게 되는 송전탑의 개수를 최대한 비슷하게 맞추고자 합니다.

송전탑의 개수 n, 그리고 전선 정보 wires가 매개변수로 주어집니다. 전선들 중 하나를 끊어서 송전탑 개수가 가능한 비슷하도록 두 전력망으로 나누었을 때, 두 전력망이 가지고 있는 송전탑 개수의 차이(절대값)를 return 하도록 solution 함수를 완성해주세요.

---

##### 제한사항

- n은 2 이상 100 이하인 자연수입니다.
- wires는 길이가 `n-1`인 정수형 2차원 배열입니다.
    - wires의 각 원소는 [v1, v2] 2개의 자연수로 이루어져 있으며, 이는 전력망의 v1번 송전탑과 v2번 송전탑이 전선으로 연결되어 있다는 것을 의미합니다.
    - 1 ≤ v1 < v2 ≤ n 입니다.
    - 전력망 네트워크가 하나의 트리 형태가 아닌 경우는 입력으로 주어지지 않습니다.

---

##### 입출력 예

|n|wires|result|
|---|---|---|
|9|`[[1,3],[2,3],[3,4],[4,5],[4,6],[4,7],[7,8],[7,9]]`|3|
|4|`[[1,2],[2,3],[3,4]]`|0|
|7|`[[1,2],[2,7],[3,7],[3,4],[4,5],[6,7]]`|1|

---

##### 입출력 예 설명

입출력 예 #1

- 다음 그림은 주어진 입력을 해결하는 방법 중 하나를 나타낸 것입니다.
- ![ex1.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/5b8a0dcd-cba0-47ca-b5e3-d3bafc81f9d6/ex1.png)
- 4번과 7번을 연결하는 전선을 끊으면 두 전력망은 각 6개와 3개의 송전탑을 가지며, 이보다 더 비슷한 개수로 전력망을 나눌 수 없습니다.
- 또 다른 방법으로는 3번과 4번을 연결하는 전선을 끊어도 최선의 정답을 도출할 수 있습니다.

입출력 예 #2

- 다음 그림은 주어진 입력을 해결하는 방법을 나타낸 것입니다.
- ![ex2.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/b28865e1-a18e-429d-ae7a-14e77e801539/ex2.png)
- 2번과 3번을 연결하는 전선을 끊으면 두 전력망이 모두 2개의 송전탑을 가지게 되며, 이 방법이 최선입니다.

입출력 예 #3

- 다음 그림은 주어진 입력을 해결하는 방법을 나타낸 것입니다.
- ![ex3.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/0a7f21af-1e07-4015-8ad3-c06155c613b3/ex3.png)
- 3번과 7번을 연결하는 전선을 끊으면 두 전력망이 각각 4개와 3개의 송전탑을 가지게 되며, 이 방법이 최선입니다.

---

### 💡 풀이

- 각 전선들은 서로 이어져있으므로 양방향 인접 리스트를 구성하여 줍니다.
- 첫 번째 예제의 경우, \[\[1,3],\[2,3],\[3,4],\[4,5],\[4,6],\[4,7],\[7,8],\[7,9]\]
- wireList.get(1) = \[3\]
- wireList.get(3) = \[1, 2, 4\]
- 이런 식으로 입력이 되겠죠!
- 그리고 전선을 끊는 작업을 해주어야 하는데, 이것은 for문을 돌면서 해당 인덱스의 전선들을 리스트에서 서로 제거해주었습니다.
- 첫 번째 루프에서는 \[1, 3\]의 연결을 끊어주는 것이니까 `wireList.get(1).remove(Integer.valueOf(3))` 처럼 되는 것이죠!
- 그 후에 완전 탐색을 해서 값을 비교해주었고 다음 루프에서는 첫 번째 루프에서 삭제해준 연결을 다시 이어주어야 하므로 아래와 같이 루프를 구성하였습니다.

```java
for (int i = 0; i < wires.length; i++) {
	visit = new boolean[n+1];
	removeWires(i, wires);
	bfs(i, wires, visit);
	addWiresIntoList(i, wires);
}
```

- 풀면서 뭔가 하드코딩하는 기분이 들어서 좀 더 효율적인 방법이 있지 않을까 하는 생각이 들었는데 다른 분들 풀이를 찾아보니 거의 비슷하게 접근한 거 같았습니다.
- 저는 bfs를 이용했는데 dfs 풀이의 효율이 좀 더 좋게 나오는 것 같네요! 아마도 탐색당 Queue를 두 개씩 지속적으로 생성하는 게 영향이 큰 것 같습니다.


### 🔍 정답

```java
import java.util.*;

class Solution {
    static ArrayList<ArrayList<Integer>> wireList;
    static boolean[] visit;
    static int min = Integer.MAX_VALUE;
    
    public int solution(int n, int[][] wires) {
        wireList = new ArrayList<>();
        
        for (int i = 0; i <= n; i++) {
            wireList.add(new ArrayList<>()); 
        }
        
        for (int i = 0; i < wires.length; i++) {
            addWiresIntoList(i, wires);
        }
        
        for (int i = 0; i < wires.length; i++) {
            visit = new boolean[n+1];
            removeWires(i, wires);
            bfs(i, wires, visit);
            addWiresIntoList(i, wires);
        }
        
        return min;
    }
    
    public void bfs(int index, int[][] wires, boolean[] visit) {
        Queue<Integer> v1Q = new LinkedList<>();
        Queue<Integer> v2Q = new LinkedList<>();
        int v1Count = 0;
        int v2Count = 0;
        
        v1Q.offer(wires[index][0]);
        visit[wires[index][0]] = true;
        v2Q.offer(wires[index][1]);
        visit[wires[index][1]] = true;
        
        min = Math.min(min, Math.abs(counting(v1Q, v1Count) - counting(v2Q, v2Count)));
    }
    
    public int counting(Queue<Integer> q, int count) {
        while (!q.isEmpty()) {
            int value = q.poll();
            
            for (int v : wireList.get(value)) {
                if (!visit[v]) {
                    q.add(v);
                    visit[v] = true;
                    count++;
                }
            }
        }
        return count;
    }
    
    public void addWiresIntoList(int index, int[][] wires) {
        int v1 = wires[index][0];
        int v2 = wires[index][1];

        wireList.get(v1).add(v2);
        wireList.get(v2).add(v1);
    }
    
    public void removeWires(int index, int[][] wires) {
        int v1 = wires[index][0];
        int v2 = wires[index][1];
        
        wireList.get(v1).remove(Integer.valueOf(v2));
        wireList.get(v2).remove(Integer.valueOf(v1));
    }
}
```