# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/1844)

## 📝 문제

###### 문제 설명

ROR 게임은 두 팀으로 나누어서 진행하며, 상대 팀 진영을 먼저 파괴하면 이기는 게임입니다. 따라서, 각 팀은 상대 팀 진영에 최대한 빨리 도착하는 것이 유리합니다.

지금부터 당신은 한 팀의 팀원이 되어 게임을 진행하려고 합니다. 다음은 5 x 5 크기의 맵에, 당신의 캐릭터가 (행: 1, 열: 1) 위치에 있고, 상대 팀 진영은 (행: 5, 열: 5) 위치에 있는 경우의 예시입니다.

![최단거리1_sxuruo.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/dc3a1b49-13d3-4047-b6f8-6cc40b2702a7/%E1%84%8E%E1%85%AC%E1%84%83%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A5%E1%84%85%E1%85%B51_sxuruo.png)

위 그림에서 검은색 부분은 벽으로 막혀있어 갈 수 없는 길이며, 흰색 부분은 갈 수 있는 길입니다. 캐릭터가 움직일 때는 동, 서, 남, 북 방향으로 한 칸씩 이동하며, 게임 맵을 벗어난 길은 갈 수 없습니다.  
아래 예시는 캐릭터가 상대 팀 진영으로 가는 두 가지 방법을 나타내고 있습니다.

- 첫 번째 방법은 11개의 칸을 지나서 상대 팀 진영에 도착했습니다.

![최단거리2_hnjd3b.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/9d909e5a-ca95-4088-9df9-d84cb804b2b0/%E1%84%8E%E1%85%AC%E1%84%83%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A5%E1%84%85%E1%85%B52_hnjd3b.png)

- 두 번째 방법은 15개의 칸을 지나서 상대팀 진영에 도착했습니다.

![최단거리3_ntxygd.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/4b7cd629-a3c2-4e02-b748-a707211131de/%E1%84%8E%E1%85%AC%E1%84%83%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A5%E1%84%85%E1%85%B53_ntxygd.png)

위 예시에서는 첫 번째 방법보다 더 빠르게 상대팀 진영에 도착하는 방법은 없으므로, 이 방법이 상대 팀 진영으로 가는 가장 빠른 방법입니다.

만약, 상대 팀이 자신의 팀 진영 주위에 벽을 세워두었다면 상대 팀 진영에 도착하지 못할 수도 있습니다. 예를 들어, 다음과 같은 경우에 당신의 캐릭터는 상대 팀 진영에 도착할 수 없습니다.

![최단거리4_of9xfg.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d963b4bd-12e5-45da-9ca7-549e453d58a9/%E1%84%8E%E1%85%AC%E1%84%83%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A5%E1%84%85%E1%85%B54_of9xfg.png)

게임 맵의 상태 maps가 매개변수로 주어질 때, 캐릭터가 상대 팀 진영에 도착하기 위해서 지나가야 하는 칸의 개수의 **최솟값**을 return 하도록 solution 함수를 완성해주세요. 단, 상대 팀 진영에 도착할 수 없을 때는 -1을 return 해주세요.

##### 제한사항

- maps는 n x m 크기의 게임 맵의 상태가 들어있는 2차원 배열로, n과 m은 각각 1 이상 100 이하의 자연수입니다.
    - n과 m은 서로 같을 수도, 다를 수도 있지만, n과 m이 모두 1인 경우는 입력으로 주어지지 않습니다.
- maps는 0과 1로만 이루어져 있으며, 0은 벽이 있는 자리, 1은 벽이 없는 자리를 나타냅니다.
- 처음에 캐릭터는 게임 맵의 좌측 상단인 (1, 1) 위치에 있으며, 상대방 진영은 게임 맵의 우측 하단인 (n, m) 위치에 있습니다.

---

##### 입출력 예

|maps|answer|
|---|---|
|[[1,0,1,1,1],[1,0,1,0,1],[1,0,1,1,1],[1,1,1,0,1],[0,0,0,0,1]]|11|
|[[1,0,1,1,1],[1,0,1,0,1],[1,0,1,1,1],[1,1,1,0,0],[0,0,0,0,1]]|-1|

##### 입출력 예 설명

입출력 예 #1  
주어진 데이터는 다음과 같습니다.

![최단거리6_lgjvrb.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/6db71f7f-58d3-4623-9fab-7cd99fa863a5/%E1%84%8E%E1%85%AC%E1%84%83%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A5%E1%84%85%E1%85%B56_lgjvrb.png)

캐릭터가 적 팀의 진영까지 이동하는 가장 빠른 길은 다음 그림과 같습니다.

![최단거리2_hnjd3b (1).png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/d223d017-b3e2-4772-9045-a565133d45ff/%E1%84%8E%E1%85%AC%E1%84%83%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A5%E1%84%85%E1%85%B52_hnjd3b%20%281%29.png)

따라서 총 11칸을 캐릭터가 지나갔으므로 11을 return 하면 됩니다.

입출력 예 #2  
문제의 예시와 같으며, 상대 팀 진영에 도달할 방법이 없습니다. 따라서 -1을 return 합니다.

---

### 💡 풀이

- DFS만으로 풀었을 때 모든 테스트 케이스를 통과하긴 했지만 효율성 테스트를 전부 통과하지 못했다.
- 최단 거리의 경우 DFS로 풀게 되면 모든 경로를 탐색하고 사이클마다 중복으로 탐색하는 노드가 생긴다. 탐색의 범위가 크면 클 수록 낭비가 더 발생하게 되는 것이다 ㅠㅠ
- 따라서 최단 거리는 BFS로 푸는 것이 좋다! BFS는 처음 어떠한 노드를 방문했을 때가 그 노드의 최단 거리이기 때문에 다른 경로에 대한 가능성을 생각하지 않아도 되고 목적지에 처음 도달했을 때, 나중에 다른 경로로 도달했을 때 중 어떤 경로가 더 짧은지를 계산하지 않아도 되므로(무조건 먼저 도착한 경로가 최단 거리) 훨씬 효율적이다!

### ❌ 효율성 테스트 실패

```java
class Solution {
    
    static int[] dx = {0, 0, 1, -1};
    static int[] dy = {1, -1, 0, 0};
    static boolean[][] visit;
    static int min = Integer.MAX_VALUE;
    
    public int solution(int[][] maps) {
        int n = maps.length;
        int m = maps[0].length;
        
        visit = new boolean[n][m];
        visit[0][0] = true;
        
        dfs(maps, 0, 0, n, m, 1);
        
        return min == Integer.MAX_VALUE ? -1 : min;
    }
    
    public static void dfs(int[][] maps, int x, int y, int n, int m, int cnt) {
        if (x == n-1 && y == m-1) {
            min = Math.min(min, cnt);
            return;
        }
        
        for (int i = 0; i < 4; i++) {
            int cx = x + dx[i];
            int cy = y + dy[i];
            
            if (cx >= 0 && cy >= 0 && cx < n && cy < m) {
                if (!visit[cx][cy] && maps[cx][cy] == 1) {
                    visit[cx][cy] = true;
                    dfs(maps, cx, cy, n, m, cnt+1);
                    visit[cx][cy] = false;
                }
            }
        }
    }
}
```


### 🔍 정답

```java
import java.util.*;

class Solution {
    
    static int[] dx = {0, 0, 1, -1};
    static int[] dy = {1, -1, 0, 0};
    static int[][] dp;
    
    public int solution(int[][] maps) {
        int n = maps.length;
        int m = maps[0].length;
        
        dp = new int[n][m];
        for (int i = 0; i < dp.length; i++) {
            Arrays.fill(dp[i], -1);
        }
        
        bfs(maps, 0, 0);
        
        return dp[n-1][m-1];
    }
    
    public static void bfs(int[][] maps, int x, int y) {
        Queue<Location> q = new LinkedList<>();
        q.add(new Location(x, y));
        dp[x][y] = 1;
            
        while (!q.isEmpty()) {
            Location lo = q.poll();
            
            for (int i = 0; i < 4; i++) {
                int cx = lo.x + dx[i];
                int cy = lo.y + dy[i];
                
                if (cx >= 0 && cy >= 0 && cx < maps.length && cy < maps[0].length) {
                    if (dp[cx][cy] == -1 && maps[cx][cy] == 1) {
                        q.add(new Location(cx, cy));
                        dp[cx][cy] = dp[lo.x][lo.y] + 1;
                    }
                }
            }
        }
    }
    
    public static class Location {
        int x, y;
        
        public Location(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
}
```