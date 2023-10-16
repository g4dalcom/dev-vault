# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/159993)

## 📝 문제

1 x 1 크기의 칸들로 이루어진 직사각형 격자 형태의 미로에서 탈출하려고 합니다. 각 칸은 통로 또는 벽으로 구성되어 있으며, 벽으로 된 칸은 지나갈 수 없고 통로로 된 칸으로만 이동할 수 있습니다. 통로들 중 한 칸에는 미로를 빠져나가는 문이 있는데, 이 문은 레버를 당겨서만 열 수 있습니다. 레버 또한 통로들 중 한 칸에 있습니다. 따라서, 출발 지점에서 먼저 레버가 있는 칸으로 이동하여 레버를 당긴 후 미로를 빠져나가는 문이 있는 칸으로 이동하면 됩니다. 이때 아직 레버를 당기지 않았더라도 출구가 있는 칸을 지나갈 수 있습니다. 미로에서 한 칸을 이동하는데 1초가 걸린다고 할 때, 최대한 빠르게 미로를 빠져나가는데 걸리는 시간을 구하려 합니다.

미로를 나타낸 문자열 배열 `maps`가 매개변수로 주어질 때, 미로를 탈출하는데 필요한 최소 시간을 return 하는 solution 함수를 완성해주세요. 만약, 탈출할 수 없다면 -1을 return 해주세요.

---

##### 제한사항

- 5 ≤ `maps`의 길이 ≤ 100
    - 5 ≤ `maps[i]`의 길이 ≤ 100
    - `maps[i]`는 다음 5개의 문자들로만 이루어져 있습니다.
        - S : 시작 지점
        - E : 출구
        - L : 레버
        - O : 통로
        - X : 벽
    - 시작 지점과 출구, 레버는 항상 다른 곳에 존재하며 한 개씩만 존재합니다.
    - 출구는 레버가 당겨지지 않아도 지나갈 수 있으며, 모든 통로, 출구, 레버, 시작점은 여러 번 지나갈 수 있습니다.

---

##### 입출력 예

|maps|result|
|---|---|
|["SOOOL","XXXXO","OOOOO","OXXXX","OOOOE"]|16|
|["LOOXS","OOOOX","OOOOO","OOOOO","EOOOO"]|-1|

---

##### 입출력 예 설명

입출력 예 #1

주어진 문자열은 다음과 같은 미로이며

![image1](https://user-images.githubusercontent.com/62426665/214443486-cb2b84a4-afc6-4b25-8da2-645a853859f1.png)

다음과 같이 이동하면 가장 빠른 시간에 탈출할 수 있습니다.

![image2](https://user-images.githubusercontent.com/62426665/207090680-93289071-da4f-4126-9c31-066c1d4d3802.png)

4번 이동하여 레버를 당기고 출구까지 이동하면 총 16초의 시간이 걸립니다. 따라서 16을 반환합니다.

입출력 예 #2

주어진 문자열은 다음과 같은 미로입니다.

![image3](https://user-images.githubusercontent.com/62426665/214443892-1e7734e9-b4c8-49af-ba29-aa5597039617.png)

시작 지점에서 이동할 수 있는 공간이 없어서 탈출할 수 없습니다. 따라서 -1을 반환합니다.

---

### 💡 풀이

- BFS 를 이용한 최단거리 문제가 약간 응용된 버전입니다.
- dp 배열을 이용해서 움직일 때마다 몇 번째 무빙인지를 기록하는 게 기본인데 레버를 당긴 후 출구로 나가야 한다는 조건이 있기 때문에 BFS를 두 번 나누어서 돌았습니다.
- 그리고 레버를 당기기 위해 지나갔던 좌표를, 레버를 당긴 후에도 지나갈 수 있기 때문에 BFS 사이에 visit 배열을 초기화시켜주는 방식으로 구현하였습니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    int[] dx = {0, 0, 1, -1};
    int[] dy = {1, -1, 0, 0};
    boolean[][] visit;
    int[][] dp;
    
    public int solution(String[] maps) {
        char[][] map = new char[maps.length][maps[0].length()];
        for (int i = 0; i < maps.length; i++) {
            for (int j = 0; j < maps[i].length(); j++) {
                map[i][j] = maps[i].charAt(j);
            }
        }
        
        visit = new boolean[map.length][map[0].length];
        dp = new int[map.length][map[0].length];
        int sx = 0, sy = 0, lx = 0, ly = 0, ex = 0, ey = 0;
        for (int i = 0; i < map.length; i++) {
            for (int j = 0; j < map[0].length; j++) {
                if (map[i][j] == 'S') {
                    sx = i;
                    sy = j;
                } else if (map[i][j] == 'L') {
                    lx = i;
                    ly = j;
                } else if (map[i][j] == 'E') {
                    ex = i;
                    ey = j;
                }
            }
        }
        
        bfs(new Node(sx, sy), lx, ly, map);
        visit = new boolean[map.length][map[0].length];
        bfs(new Node(lx, ly), ex, ey, map);
        
        if (dp[lx][ly] == 0 || dp[ex][ey] == 0) return -1;
        else return dp[ex][ey];
    }
    
    public void bfs(Node node, int x, int y, char[][] map) {
        Queue<Node> q = new LinkedList<>();
        q.add(node);
        visit[node.x][node.y] = true;
        
        while (!q.isEmpty()) {
            Node cur = q.poll();
            
            for (int i = 0; i < 4; i++) {
                int cx = cur.x + dx[i];
                int cy = cur.y + dy[i];
                
                if (cx >= 0 && cy >= 0 && cx < map.length && cy < map[0].length) {
                    if (map[cx][cy] != 'X' && !visit[cx][cy]) {
                        visit[cx][cy] = true;
                        q.add(new Node(cx, cy));
                        dp[cx][cy] = dp[cur.x][cur.y] + 1;
                    }
                                
                    if (cx == x && cy == y) break;
                }
            }
        }
    }
    
    public class Node {
        int x, y;
        
        public Node(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
}
```