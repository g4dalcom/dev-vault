# [문제링크]()

## 📝 문제

Given a `m x n` `grid` filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

**Input:** grid = [[1,3,1],[1,5,1],[4,2,1]]
**Output:** 7
**Explanation:** Because the path 1 → 3 → 1 → 1 → 1 minimizes the sum.

**Example 2:**

**Input:** grid = [[1,2,3],[4,5,6]]
**Output:** 12

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 200`
- `0 <= grid[i][j] <= 200`

---

### 💡 풀이

- 재귀를 이용한 Top-down 방식 풀이입니다.
- 메모이제이션 없이 DFS 만으로 풀이를 하면 시간 초과가 나기 때문에 DP 배열을 이용해서 중복 연산을 줄이는 방식으로 풀었습니다!
- 가장 끝 목적지에 도달했을 때부터 해당 좌표의 value 값을 누적해서 더하는 방식이고 이미 계산된 좌표라면 그대로 값을 리턴해서 중복 연산을 하지 않도록 하였습니다.

### 🔍 정답

```java
class Solution {
    static int[][] grid;
    static int[][] dp;

    public int minPathSum(int[][] grid) {
        this.grid = grid;
        dp = new int[grid.length][grid[0].length];
        
        return move(0, 0);
    }

    public int move(int row, int col) {
        if (row == grid.length-1 && col == grid[0].length-1) return grid[row][col];

        if (dp[row][col] > 0) return dp[row][col];

        if (row+1 < grid.length && col+1 < grid[0].length) {
            dp[row][col] = Math.min(move(row+1, col), move(row, col+1)) + grid[row][col];
        } else if (row+1 >= grid.length) {
            dp[row][col] = move(row, col+1) + grid[row][col];
        } else {
            dp[row][col] = move(row+1, col) + grid[row][col];
        }

        return dp[row][col];
    }
}
```


### 💡 다른 풀이(DP)

```java
class Solution {
    public int minPathSum(int[][] grid) {
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (i == 0 && j == 0) continue;
                
                if (i == 0) grid[i][j] += grid[i][j-1];
                else if (j == 0) grid[i][j] += grid[i-1][j];
                else grid[i][j] += Math.min(grid[i][j-1], grid[i-1][j]);
            }
        }

        return grid[grid.length-1][grid[0].length-1];
    }
}
```

- Bottom-up 방식의 풀이가 어떻게 보면 훨씬 직관적일 수 있겠습니다.
- i = 0 인 좌표는 자신의 이전 값(왼쪽 값)을 누적해서 더하게 되고, j = 0인 좌표는 위쪽 값을 누적해서 더하게 됩니다.
- 그 외 좌표는 대각선 이동이 가능한 좌표인데 그곳은 위에서 올 수도 있고 왼쪽에서 올 수도 있기 때문에 둘 중 합이 더 작은 값을 누적해야 합니다.