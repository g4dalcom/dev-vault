# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/1832)

## 📝 문제

카카오내비 개발자인 제이지는 시내 중심가의 경로 탐색 알고리즘 개발 업무를 담당하고 있다. 최근 들어 보행자가 자유롭고 편리하게 걸을 수 있도록 보행자 중심의 교통 체계가 도입되면서 도심의 일부 구역은 자동차 통행이 금지되고, 일부 교차로에서는 보행자 안전을 위해 좌회전이나 우회전이 금지되기도 했다. 복잡해진 도로 환경으로 인해 기존의 경로 탐색 알고리즘을 보완해야 할 필요가 생겼다.

도시 중심가의 지도는 `m × n` 크기의 격자 모양 배열 `city_map`으로 주어진다. 자동차는 오른쪽 또는 아래 방향으로 한 칸씩 이동 가능하다.

`city_map[i][j]`에는 도로의 상황을 나타내는 값이 저장되어 있다.

- `0`인 경우에는 자동차가 자유롭게 지나갈 수 있다.
- `1`인 경우에는 자동차 통행이 금지되어 지나갈 수 없다.
- `2`인 경우는 보행자 안전을 위해 좌회전이나 우회전이 금지된다. (왼쪽에서 오던 차는 오른쪽으로만, 위에서 오던 차는 아래쪽으로만 진행 가능하다)

![example map](http://t1.kakaocdn.net/codefestival/oneway500.png "One Way Map")

도시의 도로 상태가 입력으로 주어졌을 때, 왼쪽 위의 출발점에서 오른쪽 아래 도착점까지 자동차로 이동 가능한 전체 가능한 경로 수를 출력하는 프로그램을 작성하라. 이때 가능한 경로의 수는 컴퓨터가 표현할 수 있는 정수의 범위를 넘어설 수 있으므로, 가능한 경로 수를 `20170805`로 나눈 나머지 값을 출력하라.

### 입력 형식

입력은 도시의 크기를 나타내는 `m`과 `n`, 그리고 지도를 나타내는 2차원 배열 `city_map`으로 주어진다. 제한조건은 아래와 같다.

- `1 <= m, n <= 500`
- `city_map`의 크기는 `m × n`이다.
- 배열의 모든 원소의 값은 `0`, `1`, `2` 중 하나이다.
- 출발점의 좌표는 `(0, 0)`, 도착점의 좌표는 `(m - 1, n - 1)`이다.
- 출발점과 도착점의 `city_map[i][j]` 값은 `0`이다.

### 출력 형식

출발점에서 도착점까지 이동 가능한 전체 경로의 수를 `20170805`로 나눈 나머지를 리턴한다.

### 예제 입출력

|m|n|city_map|answer|
|---|---|---|---|
|3|3|[[0, 0, 0], [0, 0, 0], [0, 0, 0]]|6|
|3|6|[[0, 2, 0, 0, 0, 2], [0, 0, 2, 0, 1, 0], [1, 0, 0, 2, 2, 0]]|2|

### 예제에 대한 설명

첫 번째 예제는 모든 도로가 제한 없이 통행 가능한 경우로, 가능한 경우의 수는 6가지이다.  
두 번째 예제는 문제 설명에 있는 그림의 경우이다. 가능한 경우의 수는 빨간 실선과 노란 점선 2가지뿐이다.

---

### 💡 풀이

- BFS나 DFS로 목표 경로에 도달할 때 카운팅을 해주는 방식으로 구현이 가능할 것 같아서 DFS로 도전해보았는데 예제 케이스는 통과가 되지만 테스트를 제출하면 실패를 합니다...

### ❌ 오답(완전 탐색)

```java
import java.util.*;

class Solution {
    static int MOD = 20170805;
    static int count = 0;
    
    public int solution(int m, int n, int[][] cityMap) {
        dfs(0, 0, 0, cityMap);
        
        return count % MOD;
    }
    
    public void dfs(int x, int y, int direction, int[][] cityMap) {
        if (cityMap[x][y] == 1) return;
        
        if (x == cityMap.length-1 && y == cityMap[0].length-1) {
            count++;
            return;
        }
        
        if (cityMap[x][y] == 2) {
            if (direction == 1) {
                if (checkPossibleMove(x+1, y, cityMap)) {
                    dfs(x+1, y, 1, cityMap);
                }
            } else if (direction == 2) {
                if (checkPossibleMove(x, y+1, cityMap)) {
                    dfs(x, y+1, 2, cityMap);
                }
            }
        } else {
            if (checkPossibleMove(x+1, y, cityMap)) {
                dfs(x+1, y, 1, cityMap);
            }
            if (checkPossibleMove(x, y+1, cityMap)) {
                dfs(x, y+1, 2, cityMap);
            }
        }    
    }
    
    public boolean checkPossibleMove(int x, int y, int[][] cityMap) {
        if (x >= 0 && y >= 0 && x < cityMap.length && y < cityMap[0].length) {
            return true;
        }
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

- 이후에 생각했던 방식은 DFS를 활용하되 지나가는 경로의 개수를 DP 배열에 저장하는 방식으로 풀어보려고 하였습니다.

### ❌ 오답 (DFS + DP)

```java
class Solution {
    static int MOD = 20170805;
    static int[][] dp;
    
    public int solution(int m, int n, int[][] cityMap) {
        dp = new int[m+1][n+1];
        return dfs(0, 0, 0, cityMap);
    }
    
    public int dfs(int x, int y, int direction, int[][] cityMap) {
        if (x < 0 || y < 0 || x >= cityMap.length || y >= cityMap[0].length) return 0;
        
        if (x == cityMap.length-1 && y == cityMap[0].length-1) return 1;
        
        if (dp[x][y] > 0) return dp[x][y];
        
        if (cityMap[x][y] == 1) return 0;
        
        if (cityMap[x][y] == 2) {
            if (direction == 1) {
                dp[x][y] += dfs(x+1, y, 1, cityMap) % MOD;
            } else if (direction == 2) {
                dp[x][y] += dfs(x, y+1, 2, cityMap) % MOD;
            }
        } else {
            dp[x][y] += (dfs(x+1, y, 1, cityMap) + dfs(x, y+1, 2, cityMap)) % MOD;
        }
        
        return dp[x][y] % MOD;
    }
}
```

- 그런데 이 방법은 생각보다 경로의 개수가 더 출력이 되는 문제가 있었는데 이유는 아래와 같습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Ftf5ji%2FbtsASSzQaW3%2FAPEpkSdRcRwKZIlrdQxKrK%2Fimg.png)

- 메모이제이션을 통해서 한 번 지나갔던 경로(dp값이 0보다 큰 경우)는 해당 dp값을 반환하도록 되어있습니다.
- dp\[1\]\[2\] 의 경우 경로가 한 번 있으니 1로 초기화가 되었겠죠?
- 그런데 보라색 경로(1, 1)로 들어가면 1, 2 좌표에 진입은 되지만 결국 끝에 가서는 target에 도착할 수 없어서 0이 리턴됩니다. 재귀를 통해서 계속 리턴되다보면 dp\[1\]\[1\] 이 받는 값은 dp\[1\]\[2\] 가 가지고 있는 1을 반환받게 되는 것이죠...
- 따라서 방향성이 있는 이러한 복잡한 조건의 경우는 위 방법으로 해결할 수 없었습니다.

### 🔍 정답

```java
class Solution {
    static int MOD = 20170805;
    
    public int solution(int m, int n, int[][] cityMap) {
        int[][][] dp = new int[2][m+1][n+1];
        dp[0][0][0] = 1;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (cityMap[i][j] == 0) {
                    dp[0][i+1][j] += (dp[0][i][j] + dp[1][i][j]) % MOD;
                    dp[1][i][j+1] += (dp[0][i][j] + dp[1][i][j]) % MOD;
                } else if (cityMap[i][j] == 2) {
                    dp[0][i+1][j] += dp[0][i][j] % MOD;
                    dp[1][i][j+1] += dp[1][i][j] % MOD;
                }
            }
        }
        
        return (dp[0][m-1][n-1] + dp[1][m-1][n-1]) % MOD;
    }
}
```

- 방법은 생각보다 단순했습니다... 애시당초 dp로 접근하면 되는 것이었죠 ㅠㅠ
- 3차원 배열의 첫 번째 배열은 0과 1 두 가지로 방향성을 나타내고 cityMap == 1 인 경우에는 양방향 모두의 경로 개수를 더해주고 cityMap == 2인 경우에는 해당 방향의 경우의 수를 더해주는 풀이방식입니다!