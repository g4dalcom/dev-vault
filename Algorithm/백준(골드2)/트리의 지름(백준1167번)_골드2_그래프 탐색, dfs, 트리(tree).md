# [문제링크](https://www.acmicpc.net/problem/1167)

## 📝 문제

트리의 지름이란, 트리에서 임의의 두 점 사이의 거리 중 가장 긴 것을 말한다. 트리의 지름을 구하는 프로그램을 작성하시오.

## 입력

트리가 입력으로 주어진다. 먼저 첫 번째 줄에서는 트리의 정점의 개수 V가 주어지고 (2 ≤ V ≤ 100,000)둘째 줄부터 V개의 줄에 걸쳐 간선의 정보가 다음과 같이 주어진다. 정점 번호는 1부터 V까지 매겨져 있다.

먼저 정점 번호가 주어지고, 이어서 연결된 간선의 정보를 의미하는 정수가 두 개씩 주어지는데, 하나는 정점번호, 다른 하나는 그 정점까지의 거리이다. 예를 들어 네 번째 줄의 경우 정점 3은 정점 1과 거리가 2인 간선으로 연결되어 있고, 정점 4와는 거리가 3인 간선으로 연결되어 있는 것을 보여준다. 각 줄의 마지막에는 -1이 입력으로 주어진다. 주어지는 거리는 모두 10,000 이하의 자연수이다.

## 출력

첫째 줄에 트리의 지름을 출력한다.

## 예제 입력 1 

5
1 3 2 -1
2 4 4 -1
3 1 2 4 3 -1
4 2 4 3 3 5 6 -1
5 4 6 -1

## 예제 출력 1 

11

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int V;
    static ArrayList<ArrayList<Node>> tree;
    static boolean[] visit;
    static int maxLength = 0;
    static int max = Integer.MIN_VALUE;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        V = Integer.parseInt(br.readLine());

        tree = new ArrayList<>();
        for (int i = 0; i <= V; i++) {
            tree.add(new ArrayList<>());
        }

        // 입력 받기
        for (int i = 0; i < V; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());

            int vertex = Integer.parseInt(st.nextToken());

            while (true) {
                int edge = Integer.parseInt(st.nextToken());
                if (edge == -1) break;

                int distance = Integer.parseInt(st.nextToken());

                tree.get(vertex).add(new Node(edge, distance));
            }
        }

        // 임의의 정점에서 가장 먼 정점 구하기
        visit = new boolean[V+1];
        dfs(1, 0);

        // 위에서 구한 정점에서 가장 먼 정점까지의 거리 구하기
        Arrays.fill(visit, false);
        dfs(maxLength, 0);

        System.out.println(max);
    }

    public static void dfs(int num, int sum) {
        visit[num] = true;

        if (sum > max) {
            max = sum;
            maxLength = num;
        }

        for (Node node : tree.get(num)) {
            if (!visit[node.num]) {
                dfs(node.num, sum + node.distance);
            }
        }
    }

    public static class Node {
        int num, distance;

        public Node(int num, int distance) {
            this.num = num;
            this.distance = distance;
        }
    }
}
```
- [트리의 지름(다른 문제)](https://g4daclom.tistory.com/176)
- 이전에 풀었던 위 링크 문제와 거의 같은 문제이나 문제의 접근 방식은 좀 다르다.
- 이전에 풀었던 문제는 노드의 개수가 1만 개이고 가중치가 최대 100이었기 때문에 리프 노드를 전부 탐색하면서 가장 거리가 먼 정점의 길이(지름)를 구했다.
- 그러나 이 문제는 노드의 개수가 10만 개이고 가중치가 10,000이어서 같은 방식으로 풀면 시간 초과가 난다. 따라서 전부 탐색이 아닌 다른 방법을 찾아보아야 한다.
- [참고 풀이](https://moonsbeen.tistory.com/101)
- 참고 풀이에 따르면,
	- 임의의 정점에서 가장 거리가 먼 정점을 구하고
	- 위에서 구한 정점에서 가장 거리가 먼 정점을 구하는 것

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbgue3l%2FbtsddTufIqw%2FBfvjmMFypegjDjEK4pkExK%2Fimg.png)
- 예제를 예로 들면 정점 간 가장 거리가 먼 것은 1과 5이다.
- 이 때 다른 정점들 또한 가장 긴 정점들 중 한 점을 포함한다는 규칙이 있다.
- 따라서 임의의 아무 점에서 가장 거리가 먼 정점을 구하면 **트리상에서 가장 거리가 먼 정점 두 개 중 하나**를 알 수가 있는 것이다!
- 그러면 그 점에서 다시 가장 거리가 먼 정점을 구하면 그 점이 지름이 되는 것이다!
- 이렇게 되면 트리에서 정점이 2개이든 10,000개이든 상관없이 두 번만에 지름을 구할 수 있다.