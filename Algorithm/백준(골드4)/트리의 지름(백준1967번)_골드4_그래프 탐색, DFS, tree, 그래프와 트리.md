# [문제링크](https://www.acmicpc.net/problem/1967)

## 📝 문제

트리(tree)는 사이클이 없는 무방향 그래프이다. 트리에서는 어떤 두 노드를 선택해도 둘 사이에 경로가 항상 하나만 존재하게 된다. 트리에서 어떤 두 노드를 선택해서 양쪽으로 쫙 당길 때, 가장 길게 늘어나는 경우가 있을 것이다. 이럴 때 트리의 모든 노드들은 이 두 노드를 지름의 끝 점으로 하는 원 안에 들어가게 된다.

![](https://www.acmicpc.net/JudgeOnline/upload/201007/ttrrtrtr.png)

이런 두 노드 사이의 경로의 길이를 트리의 지름이라고 한다. 정확히 정의하자면 트리에 존재하는 모든 경로들 중에서 가장 긴 것의 길이를 말한다.

입력으로 루트가 있는 트리를 가중치가 있는 간선들로 줄 때, 트리의 지름을 구해서 출력하는 프로그램을 작성하시오. 아래와 같은 트리가 주어진다면 트리의 지름은 45가 된다.

![](https://www.acmicpc.net/JudgeOnline/upload/201007/tttttt.png)

트리의 노드는 1부터 n까지 번호가 매겨져 있다.

## 입력

파일의 첫 번째 줄은 노드의 개수 n(1 ≤ n ≤ 10,000)이다. 둘째 줄부터 n-1개의 줄에 각 간선에 대한 정보가 들어온다. 간선에 대한 정보는 세 개의 정수로 이루어져 있다. 첫 번째 정수는 간선이 연결하는 두 노드 중 부모 노드의 번호를 나타내고, 두 번째 정수는 자식 노드를, 세 번째 정수는 간선의 가중치를 나타낸다. 간선에 대한 정보는 부모 노드의 번호가 작은 것이 먼저 입력되고, 부모 노드의 번호가 같으면 자식 노드의 번호가 작은 것이 먼저 입력된다. 루트 노드의 번호는 항상 1이라고 가정하며, 간선의 가중치는 100보다 크지 않은 양의 정수이다.

## 출력

첫째 줄에 트리의 지름을 출력한다.

## 예제 입력 1 

12
1 2 3
1 3 2
2 4 5
3 5 11
3 6 9
4 7 1
4 8 7
5 9 15
5 10 4
6 11 6
6 12 10

## 예제 출력 1 

45

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int n;
    static ArrayList<ArrayList<Node>> tree;
    static boolean[] visit;
    static int max = 0;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        n = Integer.parseInt(br.readLine());

        tree = new ArrayList<>();

        for (int i = 0; i <= n; i++) {
            tree.add(new ArrayList<>());
        }

        // 양방향 인접 리스트
        for (int i = 0; i < n - 1; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int p = Integer.parseInt(st.nextToken());   // Parent
            int c = Integer.parseInt(st.nextToken());   // Child
            int w = Integer.parseInt(st.nextToken());   // Weight

            tree.get(p).add(new Node(c, w));
            tree.get(c).add(new Node(p, w));
        }

        /**
         * 리프 노드만 탐색하면 되는데
         * 양방향 인접 리스트이므로 list.size() == 1 인 경우가 리프 노드가 된다!(부모 노드만 입력되므로)
         */
        for (int i = 1; i <= n; i++) {
            if (tree.get(i).size() == 1) {
                visit = new boolean[n+1];
                visit[i] = true;
                dfs(i, 0);
            }
        }

        System.out.println(max);
    }

    public static void dfs(int num, int sum) {
        for (Node node : tree.get(num)) {
            if (!visit[node.num]) {
                visit[node.num] = true;
                dfs(node.num, sum + node.weight);
            }
        }
        max = Math.max(max, sum);
    }

    public static class Node {
        int num, weight;

        public Node(final int num, final int weight) {
            this.num = num;
            this.weight = weight;
        }
    }
}
```
- 리프 노드에서 다른 리프 노드로 가는 최대 비용을 구하는 문제이다!
- 우선 양방향 인접 리스트로 연결된 노드들을 입력하고 리프 노드인 경우에만 탐색하도록 하였다.


### 참고

- 하나의 정점에서 다른 정점으로의 최대 비용을 구하는 문제라서 다익스트라 알고리즘을 사용해도 되지 않을까라는 생각이 들어서 찾아보았는데 나와 비슷한 의문을 가진 분의 글과 참고하면 좋을 답변이 있었다!
- [이 문제 다익스트라로 풀면 안되나요???](https://www.acmicpc.net/board/view/59780)
- 다익스트라의 경우는 가중치가 모두 다르고 여러 개의 경로 중에 최단 경로를 선택해서 갱신하는 형태이다. 따라서 일반 그래프를 탐색할 때 사용할 수 있다.
- 그러나 트리는 경로가 단 하나만 존재해서 거리를 갱신하는 과정이 없기 때문에 DFS나 BFS를 이용하는 것이 더 효율적이다. 게다가 트리의 경우라면 가중치 여부와 상관없이 DFS와 BFS 모두 가능하다.

### 정리!!

###### 그래프
- 간선의 가중치가 다를 때 : 다익스트라, DFS
- 간선의 가중치가 같을 때 : BFS

###### 트리
- 가중치 유무에 상관없이 BFS, DFS 모두 가능
- 두 정점을 잇는 경로가 유일하고 거리가 갱신되는 과정이 없기 때문


#### 그래프와 트리

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FNOsIv%2FbtsaShw1gFr%2FFCxui1INNiJ5N4vwYNK5rk%2Fimg.png)

##### 그래프
- 그래프가 트리를 포함하는 더 넓은 개념
- 노드와 노드 간을 연결하는 간선으로 구성된 자료 구조
- 2개 이상의 경로가 가능하다.
	- 노드 1에서 노드 2로 가는 경로가 1 -> 2, 1 -> 3 -> 2, 1 -> 3 -> 5 -> 2 등 여러 경로가 가능

##### 트리
- 부모-자식 관계라는 레벨이 존재한다.
- 노드가 N개이면 간선은 N-1개가 된다.
- 단 하나의 경로만을 갖는다.
	- 노드 1에서 노드 5로 가는 경로는 1 -> 2 -> 5 라는 경로 하나만 존재