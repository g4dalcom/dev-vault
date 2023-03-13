# [문제링크](https://www.acmicpc.net/problem/1707)

## 📝 문제

그래프의 정점의 집합을 둘로 분할하여, 각 집합에 속한 정점끼리는 서로 인접하지 않도록 분할할 수 있을 때, 그러한 그래프를 특별히 이분 그래프 (Bipartite Graph) 라 부른다.

그래프가 입력으로 주어졌을 때, 이 그래프가 이분 그래프인지 아닌지 판별하는 프로그램을 작성하시오.

## 입력

입력은 여러 개의 테스트 케이스로 구성되어 있는데, 첫째 줄에 테스트 케이스의 개수 K가 주어진다. 각 테스트 케이스의 첫째 줄에는 그래프의 정점의 개수 V와 간선의 개수 E가 빈 칸을 사이에 두고 순서대로 주어진다. 각 정점에는 1부터 V까지 차례로 번호가 붙어 있다. 이어서 둘째 줄부터 E개의 줄에 걸쳐 간선에 대한 정보가 주어지는데, 각 줄에 인접한 두 정점의 번호 u, v (u ≠ v)가 빈 칸을 사이에 두고 주어진다. 

## 출력

K개의 줄에 걸쳐 입력으로 주어진 그래프가 이분 그래프이면 YES, 아니면 NO를 순서대로 출력한다.

## 제한

-   2 ≤ K ≤ 5
-   1 ≤ V ≤ 20,000
-   1 ≤ E ≤ 200,000

## 예제 입력 1 

2
3 2
1 3
2 3
4 4
1 2
2 3
3 4
4 2

## 예제 출력 1 

YES
NO

---

### 🔍 정답(BFS)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
    static int V, E;
    static ArrayList<Integer>[] graph;
    static int[] check;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());    // 테스트 케이스 개수

        while (T-- > 0) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            V = Integer.parseInt(st.nextToken());   // 정점의 개수
            E = Integer.parseInt(st.nextToken());   // 간선의 개수

            graph = new ArrayList[V];   // 간선의 정보를 담을 리스트
            check = new int[V];         // 기본값은 0이고 1과 -1로 그룹을 구분할 배열

            for (int i = 0; i < V; i++) {
                graph[i] = new ArrayList<>();
            }

            // 연결된 간선들을 서로의 리스트에 담아준다.
            for (int i = 0; i < E; i++) {
                st = new StringTokenizer(br.readLine());
                int a = Integer.parseInt(st.nextToken()) - 1;
                int b = Integer.parseInt(st.nextToken()) - 1;

                graph[a].add(b);
                graph[b].add(a);
            }

            // 탐색하지 않았다면(check[i] == 0), bfs 호출!
            boolean flag = true;
            for (int i = 0; i < V; i++) {
                if (check[i] == 0) {
                    if (!bfs(i)) {
                        flag = false;
                        break;
                    }
                }
            }
            System.out.println(flag ? "YES" : "NO");
        }
    }

    public static boolean bfs(int node) {
        Queue<Integer> q = new LinkedList<>();
        check[node] = 1;
        q.offer(node);

        /**
         * 현재 노드와 이어진 간선들을 탐색하는데
         * 이어진 간선 중 처음 탐색하는 곳이라면 현재 노드와 반대 부호인 수를 넣어주고(1, -1)
         * 탐색했던 곳이라면 현재 노드와 비교한다. 만약 같다면 이분 그래프가 아닌 것이다.
         */
        while (!q.isEmpty()) {
            int cur = q.poll();

            for (int i : graph[cur]) {
                if (check[i] == 0) {
                    check[i] = check[cur] * (-1);
                    q.offer(i);
                } else if (check[i] == check[cur]) {
                    return false;
                }
            }
        }
        return true;
    }
}
```

### 🔎 정답(DFS)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.StringTokenizer;

public class Main {
    static int V, E;
    static ArrayList<Integer>[] graph;
    static int[] check;
    static boolean flag;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());    // 테스트 케이스 개수

        while (T-- > 0) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            V = Integer.parseInt(st.nextToken());   // 정점의 개수
            E = Integer.parseInt(st.nextToken());   // 간선의 개수

            graph = new ArrayList[V];   // 간선의 정보를 담을 리스트
            check = new int[V];         // 기본값은 0이고 1과 -1로 그룹을 구분할 배열

            for (int i = 0; i < V; i++) {
                graph[i] = new ArrayList<>();
            }

            // 연결된 간선들을 서로의 리스트에 담아준다.
            for (int i = 0; i < E; i++) {
                st = new StringTokenizer(br.readLine());
                int a = Integer.parseInt(st.nextToken()) - 1;
                int b = Integer.parseInt(st.nextToken()) - 1;

                graph[a].add(b);
                graph[b].add(a);
            }

            flag = true;
            for (int i = 0; i < V; i++) {
                if (check[i] == 0) {
                    dfs(i, 1);
                }
            }
            System.out.println(flag ? "YES" : "NO");
        }
    }
    public static void dfs(int node, int group) {

        check[node] = group;

        for (int i : graph[node]) {
            if (check[i] == 0) {
                dfs(i, group * (-1));
            } else {
                if (check[i] == check[node]) {
                    flag = false;
                    return;
                }
            }
        }
    }
}
```
- 처음에 이분 그래프에 대해 잘못 이해를 해서 유니온파인드 알고리즘을 이용해서 해결하려고 삽질을 했었다 ㅠㅠ 모르는 개념이 있으면 일단 개념부터 확실하게 하고 풀이에 들어가는 게 맞는 것 같다.

## 이분 그래프

### 정의

- 이분 그래프는 정점을 두 그룹으로 나누었을 때 같은 그룹의 정점끼리는 인접하지 않은 그래프이다. (??) 그림으로 확인해보면 아래와 같다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbYBlP0%2Fbtr3GUa6G5k%2F5IPRQs6LaFcBu9k9Z6YrUk%2Fimg.png)
- 빨간색 그룹과 파란색 그룹으로 나누어져있는데 빨간색은 서로 인접하지 않고 파란색 또한 서로 인접해있지 않다.
- 간선이 없이 정점만 있는 경우도 이분 그래프라고 한다!


### 예시

- 이분 그래프는 굉장히 쓰임이 많은 자료구조라고 한다.
	- 학생-수업의 관계 : 학생들이 어떤 수업을 듣고 있는지를 나타내는 관계 맵
	- 시청자-드라마의 관계 : 각 시청자가 어떤 드라마를 선호하는지 나타내는 맵


### 구현

- DFS와 BFS로 모두 구현이 가능하다.
- 간선들을 각 정점들에 담아주고 하나씩 꺼냈을 때 현재 정점과 같은 색이면 이분 그래프가 아닌 것이다.
- 따라서 하나의 정점에서 시작해서 간선들을 꺼내며 방문하지 않은 곳은 현재 정점과 반대 부호를 주고 방문한 곳이라면 현재 정점과 비교해서 같다면 이분 그래프가 아니라고 출력해주면 되는 것이다!