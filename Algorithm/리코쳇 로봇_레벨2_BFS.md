# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/169199)

## 📝 문제

리코쳇 로봇이라는 보드게임이 있습니다.

이 보드게임은 격자모양 게임판 위에서 말을 움직이는 게임으로, 시작 위치에서 목표 위치까지 최소 몇 번만에 도달할 수 있는지 말하는 게임입니다.

이 게임에서 말의 움직임은 상, 하, 좌, 우 4방향 중 하나를 선택해서 게임판 위의 장애물이나 맨 끝에 부딪힐 때까지 미끄러져 이동하는 것을 한 번의 이동으로 칩니다.

다음은 보드게임판을 나타낸 예시입니다.

```
...D..R
.D.G...
....D.D
D....D.
..D....
```

여기서 "."은 빈 공간을, "R"은 로봇의 처음 위치를, "D"는 장애물의 위치를, "G"는 목표지점을 나타냅니다.  
위 예시에서는 "R" 위치에서 아래, 왼쪽, 위, 왼쪽, 아래, 오른쪽, 위 순서로 움직이면 7번 만에 "G" 위치에 멈춰 설 수 있으며, 이것이 최소 움직임 중 하나입니다.

게임판의 상태를 나타내는 문자열 배열 `board`가 주어졌을 때, 말이 목표위치에 도달하는데 최소 몇 번 이동해야 하는지 return 하는 solution함수를 완성하세요. 만약 목표위치에 도달할 수 없다면 -1을 return 해주세요.

---

##### 제한 사항

- 3 ≤ `board`의 길이 ≤ 100
    - 3 ≤ `board`의 원소의 길이 ≤ 100
    - `board`의 원소의 길이는 모두 동일합니다.
    - 문자열은 ".", "D", "R", "G"로만 구성되어 있으며 각각 빈 공간, 장애물, 로봇의 처음 위치, 목표 지점을 나타냅니다.
    - "R"과 "G"는 한 번씩 등장합니다.

---

##### 입출력 예

|board|result|
|---|---|
|["...D..R", ".D.G...", "....D.D", "D....D.", "..D...."]|7|
|[".D.R", "....", ".G..", "...D"]|-1|

---

##### 입출력 예 설명

입출력 예 #1

- 문제 설명의 예시와 같습니다.

입출력 예 #2

```
.D.R
....
.G..
...D
```

- "R" 위치에 있는 말을 어떻게 움직여도 "G" 에 도달시킬 수 없습니다.
- 따라서 -1을 return 합니다.

---

### 💡 풀이


### 🔍 정답

```java

```


### ❌ 시행착오

```java
import java.util.*;

class Solution {
    static int[] dx = {0, 0, 1, -1};
    static int[] dy = {1, -1, 0, 0};
    static char[][] ricochet;
    static int[][] dp;
    
    public int solution(String[] board) {
        ricochet = new char[board.length][board[0].length()];
        dp = new int[board.length][board[0].length()];
        for (int i = 0; i < dp.length; i++) {
            Arrays.fill(dp[i], Integer.MAX_VALUE);
        }
        int startX = 0;
        int startY = 0;
        int endX = 0;
        int endY = 0;
        
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length(); j++) {
                char current = board[i].charAt(j);
                ricochet[i][j] = current;
                if (current == 'R') {
                    startX = i;
                    startY = j;
                } else if (current == 'G') {
                    endX = i;
                    endY = j;
                }
            }
        }
        moving(new Node(startX, startY), 0);
        
        return dp[endX][endY] == Integer.MAX_VALUE ? -1 : dp[endX][endY];
    }
    
    public void moving(Node node, int count) {        
        if (dp[node.x][node.y] <= count) return;
        else dp[node.x][node.y] = count;
        
        if (ricochet[node.x][node.x] == 'G') return;
        
        for (int i = 0; i < 4; i++) {
            boolean flag = false;
            int cx = node.x;
            int cy = node.y;
            
            while (isPossibleMove(cx + dx[i], cy + dy[i])) {
                flag = true;
                cx += dx[i];
                cy += dy[i];
            }  
            if (flag) moving(new Node(cx, cy), count + 1);
        }
    }
    
    public boolean isPossibleMove(int x, int y) {
        if (x >= 0 && y >= 0 && x < ricochet.length && y < ricochet[0].length)
            return ricochet[x][y] != 'D';
        else 
            return false;
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