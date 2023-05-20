Given an `m x n` `matrix`, return _all elements of the_ `matrix` _in spiral order_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

**Input:** matrix = [[1,2,3],[4,5,6],[7,8,9]]
**Output:** [1,2,3,6,9,8,7,4,5]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)

**Input:** matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
**Output:** [1,2,3,4,8,12,11,10,9,5,6,7]

**Constraints:**

-   `m == matrix.length`
-   `n == matrix[i].length`
-   `1 <= m, n <= 10`
-   `-100 <= matrix[i][j] <= 100`

---

```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        int N = matrix.length;
        int M = matrix[0].length;

        List<Integer> result = new ArrayList<>();
        boolean[][] visit = new boolean[N][M];
        visit[0][0] = true;
        result.add(matrix[0][0]);

        int[] dx = {0, 1, -0, -1};
        int[] dy = {1, 0, -1, 0};
        int cx = 0;
        int cy = 0;

        int d = 0;
        while (result.size() < N * M) {
            d = d % 4;

            if (cx+dx[d] < N && cy+dy[d] < M && cx+dx[d] >= 0 && cy+dy[d] >= 0 && !visit[cx+dx[d]][cy+dy[d]]) {

                cx += dx[d];
                cy += dy[d];
                visit[cx][cy] = true;
                result.add(matrix[cx][cy]);

            } else d++;
        }
        return result;
    }
}
```


```java
public class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        
        List<Integer> res = new ArrayList<Integer>();
        
        if (matrix.length == 0) {
            return res;
        }
        
        int rowBegin = 0;
        int rowEnd = matrix.length-1;
        int colBegin = 0;
        int colEnd = matrix[0].length - 1;
        
        while (rowBegin <= rowEnd && colBegin <= colEnd) {
            // Traverse Right
            for (int j = colBegin; j <= colEnd; j ++) {
                res.add(matrix[rowBegin][j]);
            }
            rowBegin++;
            
            // Traverse Down
            for (int j = rowBegin; j <= rowEnd; j ++) {
                res.add(matrix[j][colEnd]);
            }
            colEnd--;
            
            if (rowBegin <= rowEnd) {
                // Traverse Left
                for (int j = colEnd; j >= colBegin; j --) {
                    res.add(matrix[rowEnd][j]);
                }
            }
            rowEnd--;
            
            if (colBegin <= colEnd) {
                // Traver Up
                for (int j = rowEnd; j >= rowBegin; j --) {
                    res.add(matrix[j][colBegin]);
                }
            }
            colBegin ++;
        }
        
        return res;
    }
}
```