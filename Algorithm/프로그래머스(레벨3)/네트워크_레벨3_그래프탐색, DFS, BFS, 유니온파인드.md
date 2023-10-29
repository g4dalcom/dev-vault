# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/43162)

## 📝 문제

네트워크란 컴퓨터 상호 간에 정보를 교환할 수 있도록 연결된 형태를 의미합니다. 예를 들어, 컴퓨터 A와 컴퓨터 B가 직접적으로 연결되어있고, 컴퓨터 B와 컴퓨터 C가 직접적으로 연결되어 있을 때 컴퓨터 A와 컴퓨터 C도 간접적으로 연결되어 정보를 교환할 수 있습니다. 따라서 컴퓨터 A, B, C는 모두 같은 네트워크 상에 있다고 할 수 있습니다.

컴퓨터의 개수 n, 연결에 대한 정보가 담긴 2차원 배열 computers가 매개변수로 주어질 때, 네트워크의 개수를 return 하도록 solution 함수를 작성하시오.

##### 제한사항

- 컴퓨터의 개수 n은 1 이상 200 이하인 자연수입니다.
- 각 컴퓨터는 0부터 `n-1`인 정수로 표현합니다.
- i번 컴퓨터와 j번 컴퓨터가 연결되어 있으면 computers[i][j]를 1로 표현합니다.
- computer[i][i]는 항상 1입니다.

##### 입출력 예

|n|computers|return|
|---|---|---|
|3|[[1, 1, 0], [1, 1, 0], [0, 0, 1]]|2|
|3|[[1, 1, 0], [1, 1, 1], [0, 1, 1]]|1|

##### 입출력 예 설명

예제 #1  
아래와 같이 2개의 네트워크가 있습니다.  
![image0.png](https://grepp-programmers.s3.amazonaws.com/files/ybm/5b61d6ca97/cc1e7816-b6d7-4649-98e0-e95ea2007fd7.png)

예제 #2  
아래와 같이 1개의 네트워크가 있습니다.  
![image1.png](https://grepp-programmers.s3.amazonaws.com/files/ybm/7554746da2/edb61632-59f4-4799-9154-de9ca98c9e55.png)

---

### 💡 풀이

- 기본적인 그래프 탐색 문제이자 유니온파인드를 적용할 수 있는 문제라서 여러가지 방법으로 풀이해보았습니다.
- 성능은 DFS가 가장 잘 나오고 그다음이 유니온파인드, BFS 순이었습니다.
- 그래프 탐색의 경우는 연결되어있는 컴퓨터들을 방문하면서 방문체크를 해주었고 한 번의 탐색을 할 때마다 카운팅을 해주는 방식으로 풀이하였습니다.
- 유니온파인드 알고리즘을 적용한 방법은, 연결된 컴퓨터들을 union 하면서 작은 컴퓨터를 부모로 연결해주는 방식을 사용하였고 set에 부모 노드를 넣고 set의 사이즈를 리턴하는 방식으로 풀이하였습니다.
- 그런데 유니온파인드 방식의 경우는 한 가지 주의할 점이 있었는데요!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcKy0eR%2Fbtszklcmvf4%2FuaLcXWhKso0ZFs0iTiPaf0%2Fimg.png)

- 0번부터 n-1까지 순차적으로 돌면서 유니온을 해주는데요.
- 위 케이스의 경우 0번과 6번이 직접적으로 연결되어있어서 0과 6의 부모가 0이 됩니다.
- 그리고 1번 탐색에서 2, 4의 부모가 1이 되고 이어서 3, 5의 부모도 1이 됩니다.
- 그리고 5번 탐색에서 4의 부모인 1의 부모가 0이 됩니다.
- 결국 부모 노드가 아래와 같이 나타나는데요
- 0 0
- 1 0
- 2 1
- 3 1
- 4 1
- 5 0
- 6 0
- 사실 모든 컴퓨터가 연결되어 있으므로 네트워크는 1개이고 유니온파인드를 했다면 모든 노드의 부모는 가장 작은 수인 0이 되어야 하지만 탐색의 순서 때문에 그렇지 못하게 되었죠.
- 결국 2, 3, 4의 부모가 1이 되고 0, 1, 5, 6의 부모는 0이 되어버린 것입니다.
- 그래서 모든 탐색이 끝난 후에 set에 부모 노드를 넣어줄 때 find 연산은 한 번 더 해줌으로써 탐색 이후에 바뀐 부모노드를 찾아줄 수 있었습니다!


### 🔍 정답(DFS)

```java
class Solution {
    static boolean[] visit;
    static int networkCount = 0;
    
    public int solution(int n, int[][] computers) {
        visit = new boolean[n];
        
        for (int i = 0; i < n; i++) {
            if (!visit[i]) {
                networkCount++;
                visit[i] = true;
                dfs(i, computers);
            }
        }
        
        return networkCount;
    }
    
    public void dfs(int current, int[][] computers) {
        for (int i = 0; i < computers.length; i++) {
            if (!visit[i] && computers[current][i] == 1) {
                visit[i] = true;
                dfs(i, computers);
            }
        }
    }
}
```

### 🔍 정답(BFS)

```java
import java.util.*;

class Solution {
    static List<List<Integer>> list = new ArrayList<>();
    static boolean[] visit;
    static int networkCount = 0;
    
    public int solution(int n, int[][] computers) {
        visit = new boolean[n];
        for (int i = 0; i < n; i++) {
            list.add(new ArrayList<>());
        }
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i != j && computers[i][j] == 1) {
                    list.get(i).add(j);
                }
            }
        }
        
        for (int i = 0; i < n; i++) {
            if (!visit[i]) {
                networkCount++;
                bfs(i);
            }
        }
        
        return networkCount;
    }
    
    public void bfs(int current) {
        Queue<Integer> q = new LinkedList<>();
        q.add(current);
        visit[current] = true;
        
        while (!q.isEmpty()) {
            int cur = q.poll();
            
            for (int link : list.get(cur)) {
                if (!visit[link]) {
                    visit[link] = true;
                    q.add(link);
                }
            }
        }
    }
}
```


### 🔍 정답(유니온파인드)

```java
import java.util.*;

class Solution {
    static int[] parent;
    
    public int solution(int n, int[][] computers) {
        parent = new int[n];
        
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i != j && computers[i][j] == 1) {
                    union(i, j);
                }
            }
        }
        
        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < parent.length; i++) {
            set.add(find(parent[i]));
        }
        
        return set.size();
    }
    
    public int find(int x) {
        if (parent[x] == x) return x;
        
        return parent[x] = find(parent[x]);
    }
    
    public void union(int a, int b) {
        a = find(a);
        b = find(b);
        
        if (a != b) {
            if (a < b) {
                parent[b] = a;
            } else {
                parent[a] = b;
            }
        }
    }
}
```