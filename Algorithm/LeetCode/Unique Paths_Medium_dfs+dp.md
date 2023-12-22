# [문제링크](https://leetcode.com/problems/unique-paths/)

## 📝 문제

There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return _the number of possible unique paths that the robot can take to reach the bottom-right corner_.

The test cases are generated so that the answer will be less than or equal to `2 * 109`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

**Input:** m = 3, n = 7
**Output:** 28

**Example 2:**

**Input:** m = 3, n = 2
**Output:** 3
**Explanation:** From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down

**Constraints:**

- `1 <= m, n <= 100`

---

### 💡 풀이

- dfs와 dp를 이용한 풀이입니다.
- 기본적인 접근법은, 출발지점에서 깊이 우선 탐색으로 목적지에 도달했을 때 거쳐온 경로(dp배열)에 1씩 값을 누적해주는 것입니다.
- 그러면 결국 dp\[0\]\[0\] 에는 목적지에 도달하는 경로에 대한 경우의 수가 누적되겠죠?!
- 그리고 이미 거쳐갔던 길은 다시 연산하지 않고 그동안 누적된 값을 그대로 더해주면서 효율성을 챙겼습니다!


### 🔍 정답

```java
class Solution {
    static int[][] dp;
    static int[] dx = {1, 0};
    static int[] dy = {0, 1};
    static int m, n;

    public int uniquePaths(int m, int n) {
        this.m = m;
        this.n = n;
        dp = new int[m][n];

        return dfs(0, 0);
    }

    public int dfs(int row, int col) {
        if (row == m-1 && col == n-1) return 1;
        if (dp[row][col] > 0) return dp[row][col];

        for (int i = 0; i < 2; i++) {
            int cx = row + dx[i];
            int cy = col + dy[i];

            if (cx < m && cy < n) {
                dp[row][col] += dfs(cx, cy);
            }
        }
        
        return dp[row][col];
    }
}
```