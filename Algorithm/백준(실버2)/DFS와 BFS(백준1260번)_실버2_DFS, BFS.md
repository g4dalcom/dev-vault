# [문제링크](https://www.acmicpc.net/problem/1260)

## 📝 문제

그래프를 DFS로 탐색한 결과와 BFS로 탐색한 결과를 출력하는 프로그램을 작성하시오. 단, 방문할 수 있는 정점이 여러 개인 경우에는 정점 번호가 작은 것을 먼저 방문하고, 더 이상 방문할 수 있는 점이 없는 경우 종료한다. 정점 번호는 1번부터 N번까지이다.

## 입력

첫째 줄에 정점의 개수 N(1 ≤ N ≤ 1,000), 간선의 개수 M(1 ≤ M ≤ 10,000), 탐색을 시작할 정점의 번호 V가 주어진다. 다음 M개의 줄에는 간선이 연결하는 두 정점의 번호가 주어진다. 어떤 두 정점 사이에 여러 개의 간선이 있을 수 있다. 입력으로 주어지는 간선은 양방향이다.

## 출력

첫째 줄에 DFS를 수행한 결과를, 그 다음 줄에는 BFS를 수행한 결과를 출력한다. V부터 방문된 점을 순서대로 출력하면 된다.

## 예제 입력 1 
4 5 1
1 2
1 3
1 4
2 4
3 4

## 예제 출력 1 

1 2 4 3
1 2 3 4

## 예제 입력 2

5 5 3
5 4
5 2
1 2
3 4
3 1

## 예제 출력 2 

3 1 2 5 4
3 1 4 2 5

## 예제 입력 3 

1000 1 1000
999 1000

## 예제 출력 3 

1000 999
1000 999


---

### 🔍 정답

```java
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
import java.util.LinkedList;  
import java.util.Queue;  
import java.util.StringTokenizer;  
  
public class Main {  
  
    static int N, M, V;  
    static int[][] arr;  
    static boolean[] visit;  
    static StringBuilder sb = new StringBuilder();  
  
    public static void main(String[] args) throws IOException {  
  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        StringTokenizer st = new StringTokenizer(br.readLine());  
        N = Integer.parseInt(st.nextToken()); // 정점의 개수  
        M = Integer.parseInt(st.nextToken()); // 간선의 개수  
        V = Integer.parseInt(st.nextToken()); // 시작할 정점의 번호  
  
        arr = new int[N+1][N+1]; // 이어진 간선을 1로 표시할 2차원 배열  
        visit = new boolean[N+1]; // 방문 여부를 표시할 배열  
  
        for (int i = 0; i < M; i++) {  
            st = new StringTokenizer(br.readLine());  
            int a = Integer.parseInt(st.nextToken());  
            int b = Integer.parseInt(st.nextToken());  
  
            arr[a][b] = arr[b][a] = 1;  
        }  
  
        dfs(V);  
        sb.append("\n");  
  
        visit = new boolean[N+1];  
        bfs(V);  
  
        System.out.println(sb);  
    }  
  
    public static void dfs(int V) {  
        visit[V] = true;  
        sb.append(V + " ");  
  
        for (int i = 1; i <= N; i++) {  
            if (arr[V][i] == 1 && !visit[i]) {  
                dfs(i);  
            }  
        }  
    }  
  
    public static void bfs(int V) {  
        Queue<Integer> q = new LinkedList<>();  
        q.add(V);  
        visit[V] = true;  
  
        while (!q.isEmpty()) {  
  
            V = q.poll();  
            sb.append(V + " ");  
  
            for (int i = 1; i <= N; i++) {  
                if (arr[V][i] == 1 && !visit[i]) {  
                    q.add(i);  
                    visit[i] = true;  
                }  
            }  
        }  
    }  
}
```
- dfs와 bfs의 기본 구조를 배울 수 있는 문제였음다.
- dfs는 스택, 재귀로 풀고 bfs는 큐를 이용해서 풀 수 있다고 합니다!
- 아래 조건으로 예를 들어서 풀이를 해볼게용
	- 4 5 1
	- 1 2
	- 1 3
	- 1 4
	- 2 4
	- 3 4
- DFS 풀이
	- DFS는 재귀를 이용해 풀었어요!
	- visit = [F F F F F], 편의상 0번 인덱스는 빼고 생각합니다!
	- 여기서 첫 시작점(V)을 True로 바꿔줍시다
		- visit = [F T F F F], sb = [1]
	- for문은 arr[V]\[1부터~N까지] 돌면서 아까 간선으로 입력받아서 1로 바꾼 애들을 찾습니다. 
		- arr[1]\[1] == 0 이니까 스킵하고
		- arr[1]\[2] == 1 이고 visit 2번 인덱스는 false니까 if문을 탑니다
		- dfs(2)를 실행합니다(재귀!)
	- visit = [F T T F F], sb = [1 2] 가 되고 다시 for문을 돌겠죵. 
	- arr[2]\[1부터~N까지] 도는데 arr[2]\[4] == 1이므로 dfs(4)을 부르고 visit = [F T T F T], sb = [1 2 4]
	- for문을 다시 들어가는데 arr[4]\[1부터~N까지]는 해당하는 게 없으므로 재귀 전으로 돌아갑니다(dfs(4)를 불렀던 곳)
		- arr[2]\[1부터~N까지]로 돌아가는데 얘도 arr[2]\[4]까지 다 돌았으니 재귀 전으로 돌아갑니다
			- arr[1]\[2]까지 돌다가 재귀함수를 불렀었으니까 arr[1]\[3]부터 돌겠네용
			- visit = [F T T T T], sb = [1 2 4 3]이 되면서 dfs 루프는 끝이 납니다!
- BFS 풀이
	- BFS는 큐를 이용했음다
	- 먼저 큐에 시작점(V)을 넣고 visit[V] 를 true로 바꿔줍니다. 
	- 그런 후에는 큐가 빌 때까지 반복문을 돌아줍니다
		- 큐에 있는 애를 빼서 V 변수에 넣고 for문에 들어갑니다
			- V = 1 sb = [1]
		- for문은 dfs랑 유사하지만 중간에 재귀를 부르지 않고 끝까지 돈다는 점이 차이가 있습니다!
		- arr[1]\[1부터~N까지] 중에 입력값이면서 방문하지 않은 값이 있으면 큐에 계속해서 넣어주고 visit를 true로 바꿔주는 것이죵
		- arr[1]\[2], arr[1]\[3], arr[1]\[4] 가 모두 입력값이므로
			- q = [2 3 4], visit = [F T T T T] 가 됩니당
		- V = 1이 for문을 다 돌았는데 q가 비어있지 않으니까 while문은 계속됩니다
			- 큐에서 맨 위에 있는 애를 뽑아서 V 변수에 넣어주고
				- q = [3 4], V = 2, sb = [1 2]
			- for문을 도는데 visit를 보면 전부 True가 되어있죵. 그러면 for문은 전부 스킵되고 q에 있는 애들을 하나씩 빼서 sb에 넣어준다고 예상할 수 있겠네요!
	- 예제가 이러해서 시시하게 끝이 났지만, 큐의 특성인 FIFO(선입선출)을 이용해서 시작점과 관련된 간선들을 순서대로 큐에 넣고(정점 번호가 적은 애들부터 들어갔겠죵)
	- 그 다음 정점에 해당하는 애를 기준으로 다시 반복해서 해당 정점에서 방문할 수 있는 노드를 전부 방문하고, 정점 이동하고 관련 노드 방문..(반복) 하는 식으로 생각하면 되었습니다!