# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42898)

## 📝 문제

계속되는 폭우로 일부 지역이 물에 잠겼습니다. 물에 잠기지 않은 지역을 통해 학교를 가려고 합니다. 집에서 학교까지 가는 길은 m x n 크기의 격자모양으로 나타낼 수 있습니다.

아래 그림은 m = 4, n = 3 인 경우입니다.

![image0.png](https://grepp-programmers.s3.amazonaws.com/files/ybm/056f54e618/f167a3bc-e140-4fa8-a8f8-326a99e0f567.png)

가장 왼쪽 위, 즉 집이 있는 곳의 좌표는 (1, 1)로 나타내고 가장 오른쪽 아래, 즉 학교가 있는 곳의 좌표는 (m, n)으로 나타냅니다.

격자의 크기 m, n과 물이 잠긴 지역의 좌표를 담은 2차원 배열 puddles이 매개변수로 주어집니다. **오른쪽과 아래쪽으로만 움직여** 집에서 학교까지 갈 수 있는 최단경로의 개수를 1,000,000,007로 나눈 나머지를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 격자의 크기 m, n은 1 이상 100 이하인 자연수입니다.
    - m과 n이 모두 1인 경우는 입력으로 주어지지 않습니다.
- 물에 잠긴 지역은 0개 이상 10개 이하입니다.
- 집과 학교가 물에 잠긴 경우는 입력으로 주어지지 않습니다.

##### 입출력 예

|m|n|puddles|return|
|---|---|---|---|
|4|3|[[2, 2]]|4|

##### 입출력 예 설명

![image1.png](https://grepp-programmers.s3.amazonaws.com/files/ybm/32c67958d5/729216f3-f305-4ad1-b3b0-04c2ba0b379a.png)

---

### 💡 풀이

- 기본적으로 DFS를 이용해서 목적지에 도달했을 때를 카운팅해주는 것인데, 단순히 DFS만으로 구하게 되면 경우의 수가 너무나 많기 때문에 시간 초과가 발생합니다.
- 따라서 DP 배열을 이용해서 목적지에 도달했을 때 해당 DP 좌표에 1을 누적해주고 이미 거쳐갔던 좌표라면 메모이제이션을 통해 좌푯값 그대로를 리턴해주도록 하였습니다.

```java
if (x == n && y == m) return 1;
if (dp[x][y] > 0) return dp[x][y];
```

- 이렇게 하면 목적지에 도달하는 경로의 좌표마다 해당 좌표를 거쳐간 개수가 누적되게 되며 마지막에 처음 시작한 좌표를 리턴하게 되면 목적지에 도달할 수 있는 경로의 개수가 구해집니다.
- 그리고 물에 잠긴 구역은 거쳐가지 말아야 하므로 해당 좌표를 dp 배열에 -1로 초기화해줌으로써 다른 구역과 구별되도록 하였습니다.
- 마지막으로, 최종 리턴값에만 MOD 연산을 하게 되면 효율성 테스트를 통과하지 못하게 되므로 dp배열에 값을 누적해줄 때마다 MOD 연산을 해야 합니다!

### 🔍 정답

```java
class Solution {
    static int[][] dp;
    static int[] dx = {0, 1};
    static int[] dy = {1, 0};
    static final int MOD = 1000000007;
    
    public int solution(int m, int n, int[][] puddles) {
        dp = new int[n+1][m+1];
        
        for (int i = 0; i < puddles.length; i++) {
            dp[puddles[i][1]][puddles[i][0]] = -1;
        }
        
        return dfs(1, 1, n, m) % MOD;
    }
    
    public int dfs(int x, int y, int n, int m) {
        if (x == n && y == m) return 1;
        if (dp[x][y] > 0) return dp[x][y];
    
        for (int i = 0; i < 2; i++) {
            int cx = x + dx[i];
            int cy = y + dy[i];
            
            if (cx <= n && cy <= m && dp[cx][cy] != -1) {
                dp[x][y] += dfs(cx, cy, n, m) % MOD;
            }
        }     
        return dp[x][y];
    }
}
```