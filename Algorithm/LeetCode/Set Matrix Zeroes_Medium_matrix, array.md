# [문제링크](https://leetcode.com/problems/set-matrix-zeroes/)

## 📝 문제

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

**Input:** matrix = [[1,1,1],[1,0,1],[1,1,1]]
**Output:** [[1,0,1],[0,0,0],[1,0,1]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

**Input:** matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
**Output:** [[0,0,0,0],[0,4,5,0],[0,3,1,0]]

**Constraints:**

- `m == matrix.length`
- `n == matrix[0].length`
- `1 <= m, n <= 200`
- `-231 <= matrix[i][j] <= 231 - 1`

**Follow up:**

- A straightforward solution using `O(mn)` space is probably a bad idea.
- A simple improvement uses `O(m + n)` space, but still not the best solution.
- Could you devise a constant space solution?

---

### 💡 풀이

-  2차원 배열에서 0인 좌표의 행과 열을 모두 0으로 바꾸어주어야 하는 문제입니다.
- 간단한 방법으로 0 을 만났을 때 해당 행과 열을 모두 0으로 바꿔나가는 방법은 아직 탐색하지 않은 좌표까지 0으로 바꿔버리는 심각한 오류가 있습니다.
- 이것을 해결하기 위해서 새로운 배열을 사용해서 해결할 수도 있고 해시맵을 사용할 수도 있습니다.

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        boolean[][] isZero = new boolean[matrix.length][matrix[0].length];

        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                if (matrix[i][j] == 0) {
                    isZero[i][j] = true;
                }
            }
        }

        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                if (isZero[i][j]) {
                    makeZero(i, j, matrix);
                }
            }
        }
    }

    public void makeZero(int row, int col, int[][] matrix) {
        int rc = 0;
        int cc = 0;
        while (rc < matrix[0].length) {
            matrix[row][rc] = 0;
            rc++;
        }

        while (cc < matrix.length) {
            matrix[cc][col] = 0;
            cc++;
        }
    }
}
```

- 위와 같은 방법은 새로운 배열을 이용해서 0인 좌표를 걸러내고 모든 좌표를 탐색한 이후에 0인 좌표에 해당하는 행과 열만 0으로 바꾸는 방법인데 이 방법은 문제에서 권장하는 방식은 아닙니다.

### 🔍 정답

```java
class Solution {
    public void setZeroes(int[][] matrix) {
        boolean rowZero = false;
        boolean colZero = false;

        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                if (matrix[i][j] == 0) {
                    if (i == 0) rowZero = true;
                    if (j == 0) colZero = true;

                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        for (int i = 1; i < matrix.length; i++) {
            for (int j = 1; j < matrix[0].length; j++) {
                if (matrix[i][0] == 0 || matrix[0][j] == 0) matrix[i][j] = 0;
            }
        }

        if (rowZero) {
            for (int j = 0; j < matrix[0].length; j++) {
                matrix[0][j] = 0;
            }
        }

        if (colZero) {
            for (int i = 0; i < matrix.length; i++) {
                matrix[i][0] = 0;
            }
        }
    }
}
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FRYaEr%2FbtsCG2Hlmy1%2FBpKFKP8rxbJYvaEzvJgN00%2Fimg.png)

- 위 방법은 0번 행과 0번 열을 이용합니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fp04BI%2FbtsCKRyu0aL%2FIDLx0LbJPMOiagK7gfmk6K%2Fimg.png)

- 예를 들어 (1, 2) 좌표가 0이라고 한다면 해당 행(1)의 0번도 0이 될 것이고 해당 열(2)의 0번도 0이 되겠죠?!
- 이 때 해당 열과 행의 0번은 이미 탐색한 좌표이므로 0으로 바꾸어주어도 문제가 되지 않습니다. 따라서 좌표를 탐색하며 0을 발견하면 해당 행과 열의 0번을 0으로 바꾸어주는 것입니다.

```java
for (int i = 0; i < matrix.length; i++) {
	for (int j = 0; j < matrix[0].length; j++) {
		if (matrix[i][j] == 0) {
			if (i == 0) rowZero = true;
			if (j == 0) colZero = true;

			matrix[i][0] = 0;
			matrix[0][j] = 0;
		}
	}
}
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDoEVu%2FbtsCBNxdLOG%2FFbIKFhaPVZgNf8PJbsYHi0%2Fimg.png)

- 또한 여기서 사용한 boolean 값 rowZero와 colZero는 각각 0번 행과 열이 0인지를 체크합니다. 만약 0번 중 하나라도 0이 있다면 모든 0번 행이나 열은 0이 되겠죠???

```java
if (rowZero) {
	for (int j = 0; j < matrix[0].length; j++) {
		matrix[0][j] = 0;
	}
}

if (colZero) {
	for (int i = 0; i < matrix.length; i++) {
		matrix[i][0] = 0;
	}
}
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbK03Ev%2FbtsCK9yYlMu%2FvUpTGvm6K7kAaRMHMguV40%2Fimg.png)

- 이런 식으로 0번 행과 0번 열에 0 여부를 체크해두었다가 다시 좌표를 탐색할 때 자신이 속하는 행이나 열의 0번 중 한 곳이라도 0이라면 0으로 바꾸어나가는 방식입니다.