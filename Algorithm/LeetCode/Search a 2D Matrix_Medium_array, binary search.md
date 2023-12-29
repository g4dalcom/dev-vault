# [문제링크](https://leetcode.com/problems/search-a-2d-matrix/)

## 📝 문제

You are given an `m x n` integer matrix `matrix` with the following two properties:

- Each row is sorted in non-decreasing order.
- The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, return `true` _if_ `target` _is in_ `matrix` _or_ `false` _otherwise_.

You must write a solution in `O(log(m * n))` time complexity.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

**Input:** matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg)

**Input:** matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
**Output:** false

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 100`
- `-104 <= matrix[i][j], target <= 104`

---

### 💡 풀이

- 처음 풀이는 0번 행부터 target이 행 안에 속할 가능성이 있는 수인지를 확인한 뒤에 가능한 수라면 해당 행만 탐색하여 target을 구하는 방법입니다.

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int p = 0;
        int size = matrix[0].length-1;
        boolean result = false;

        while (true) {
            if (p >= matrix.length || matrix[p][0] > target) break;

            if (matrix[p][0] <= target && matrix[p][size] >= target) {
                result = findTarget(p, matrix, target);
                break;
            }
            p++;
        }

        return result;
    }

    public boolean findTarget(int p, int[][] matrix, int target) {
        for (int i = 0; i < matrix[0].length; i++) {
            if (matrix[p][i] == target) return true;
        }

        return false;
    }
}
```

- 이 방법은 모든 배열을 탐색하지 않고 각 행의 양 끝단만을 확인한 뒤 마지막에 하나의 행만 전체 탐색하는 것이지만 효율은 좋지 않았습니다.
- 그래서 생각한 것이 이분 탐색이었는데 정렬된 1차원 배열에서는 이분 탐색이 간단했지만 2차원 배열은 좀 더 고민이 필요했습니다.
- 주요 아이디어는 **나눗셈 연산** 이었습니다.
- 전체 요소의 크기(matrix.length * matrix[0].length - 1) 를 하나의 배열이라고 생각하고 계산된 인덱스를 2차원 배열로 변환하는 것입니다.
- 예제 1번을 예로 들면, \[\[1,3,5,7\],\[10,11,16,20\],\[23,30,34,60\]\] 라는 2차원 배열에서 left = 0, right = 11(matrix.length * matrix[0].length - 1)로 두고 이분 탐색을 시작한다고 가정하겠습니다.
- 이 때 mid 값은 (0 + 11) / 2 = 5가 되겠죠?! 인덱스 5는 row와 column 어디에 두어도 범위를 벗어납니다. 하지만 나눗셈 연산을 하면 행과 열로 나눌 수도 있습니다.
- mid / matrix[0].length 를 하면 5 / 4 = 1이 되어 1번 행을 가리키게 되고 mid % matrix[0].length 를 하면 5 % 4 = 1이 되고 1번 열을 가리키게 됩니다. 따라서 matrix\[1\]\[1\] 인 11이 중간값이 되는 것이죠!

### 🔍 정답

```java
    public boolean searchMatrix(int[][] matrix, int target) {
        int left = 0;
        int right = (matrix.length * matrix[0].length) - 1;

        while (left <= right) {
            int mid = (left + right) / 2;
            int row = mid / matrix[0].length;
            int col = mid % matrix[0].length;

            if (matrix[row][col] == target) {
                return true;
            } else if (matrix[row][col] > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }

        return false;
    }
}
```