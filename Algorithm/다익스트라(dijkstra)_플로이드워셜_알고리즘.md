
# 다익스트라 알고리즘
- 단일 시작점 최단 경로 알고리즘으로 `하나의 정점`에서 출발하였을 때 `다른 모든 정점`으로의 최단 경로를 구한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcwLgzt%2FbtrYz8zokRH%2FTidJcGSkkffFUohppAgYlk%2Fimg.png)
- 1을 기준으로 잡고 최단 경로를 구한다고 할 때,
- 4의 경우 1 -> 4로 가는 비용보다 1 -> 3 -> 4로 가는 비용이 더 적기 때문에 더 적은 비용을 갱신할 수 있다.

### 구현 방법
- BFS와 유사한 형태로, 시작점에서 가까운 순서대로 정점을 방문한다. 가중치가 있는 그래프에서는 BFS를 그대로 적용하기 어렵기 때문에 우선순위큐를 사용하여 해결한다.
- 각 정점까지의 최단 거리를 저장하는 배열 dp[]를 유지하며, 정점을 방문할 때마다 인접한 정점을 모두 검사한다. 
- 간선 (u, v)를 검사한다고하면 u까지의 최단 거리에 (u, v)의 가중치를 더해 v까지의 경로의 길이를 찾는다. 만약 이 길이가 최단거리라면 dp[v]를 갱신하고, (v, dp[v])를 큐에 넣는다.

```java
	static void solution(int n , int maps[][]) {
		check = new boolean[n+1];
		dp = new int[n+1];
		
		list = new ArrayList[n+1];
		
		for(int i=1; i<n+1; i++	) {
			list[i] = new ArrayList<>();
		}
		
		for(int i=0; i<maps.length; i++) {
			int a = maps[i][0];
			int b = maps[i][1];
			int c = maps[i][2];
			
			list[a].add(new Node(b,c));
			list[b].add(new Node(a,c));
		}
		
		dijkstra(1);
	}
	
	
	static void dijkstra(int start) {
		Queue<Node> q = new PriorityQueue<>();
		Arrays.fill(dp, Integer.MAX_VALUE);
		
		q.add(new Node(start,0));
		
		dp[start] = 0;
		
		while(!q.isEmpty()) {
			Node node = q.poll();
			int to = node.to;
			
			if(check[to]) continue;
			else check[to] = true;
			
			for(Node nxt : list[to]) {
				if(dp[nxt.to]  >= dp[to] + nxt.weight) {
					dp[nxt.to] = dp[to] + nxt.weight;
					q.add(new Node(nxt.to, dp[nxt.to]));
				}
			}
		}
	}
}

class Node implements Comparable<Node>{
	int to;
	int weight;
	
	Node(int to, int weight){
		this.to = to;
		this.weight = weight;
	}

	@Override
	public int compareTo(Node o) {
	
```


# 플로이드 워셜
- `모든 정점` 에서 `다른 모든 정점`으로의 최단 경로를 계산해야 하는 경우에 사용한다.
- 매 단계마다 `현재 노드를 거쳐가는 노드`를 기준으로 2차원 리스트에 최단 거리를 저장한다.
- 3중 for문을 이용한 구현으로 시간 복잡도가 높기 때문에 그래프의 크기가 크다면 구현하기 어렵다.

### 구현 방법

```java
	static void solution(int n, int[][] arr) {
		int[][] floyd = new int[n][n];
		
		// 결과 그래프 초기화 
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				if (i == j) {
					floyd[i][j]	 =0;
				} else floyd[i][j] = 1_000_000_000;
			}
		}
		
		// 결과 그래프 입력 
		for(int i = 0; i < arr.length; i++) {
			floyd[arr[i][0]-1][arr[i][1]-1] = arr[i][2];
		}
		
		// k : 거쳐가는 노드 (기준) 
		for(int k = 0; k < n; k++) {
			// i : 출발 노드  
			for(int i = 0; i < n; i++) {
				// j : 도착 노드 
				for(int j = 0; j < n; j++) {
					// i에서 j로 가는 최소 비용 VS 
					// i에서 노드 k로 가는 비용 + 노드 k에서 jY로 가는 비용
					if(floyd[i][k] + floyd[k][j] < floyd[i][j]) {
						floyd[i][j] = floyd[i][k] + floyd[k][j];
					}
				}
			}
		}
```
- 중간 경로(k)를 거칠 때와 거치지 않을 때를 비교한다.