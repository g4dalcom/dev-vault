Given an `m x n` matrix `mat`, return _an array of all the elements of the array in a diagonal order_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/10/diag1-grid.jpg)

**Input:** mat = [[1,2,3],[4,5,6],[7,8,9]]
**Output:** [1,2,4,7,5,3,6,8,9]

**Example 2:**

**Input:** mat = [[1,2],[3,4]]
**Output:** [1,2,3,4]

**Constraints:**

-   `m == mat.length`
-   `n == mat[i].length`
-   `1 <= m, n <= 104`
-   `1 <= m * n <= 104`
-   `-105 <= mat[i][j] <= 105`

---

```java
class Solution {
    public int[] findDiagonalOrder(int[][] mat) {
        int m = mat.length;
        int n = mat[0].length;
        
        int[] result = new int[m * n];

        int i = 0;
        int j = 0;

        for (int k = 0; k < result.length; k++) {
            result[k] = mat[i][j];
            if ((i + j) % 2 == 0) {
                if (j == n - 1) i++;
                else if (i == 0) j++;
                else {
                    i--;
                    j++;
                }
            } else {
                if (i == m - 1) j++;
                else if (j == 0) i++;
                else {
                    i++;
                    j--;
                }
            }
        }
        return result;
    }
}  
```