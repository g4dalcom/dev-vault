# [문제링크](https://leetcode.com/problems/rotate-image/)

## 📝 문제

You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/28/mat1.jpg)

**Input:** matrix = [[1,2,3],[4,5,6],[7,8,9]]
**Output:** [[7,4,1],[8,5,2],[9,6,3]]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/28/mat2.jpg)

**Input:** matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
**Output:** [[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]

**Constraints:**

- `n == matrix.length == matrix[i].length`
- `1 <= n <= 20`
- `-1000 <= matrix[i][j] <= 1000`

---

### 💡 풀이

- 문제의 조건으로 새로운 배열을 생성하지 말라고 되어 있습니다.

```java
class Solution {
    public void rotate(int[][] matrix) {
        int[][] copy = new int[matrix.length][matrix[0].length];
        for (int i = 0; i < matrix.length; i++) {
            copy[i] = matrix[i].clone();
        }

        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[0].length; j++) {
                int k = matrix.length - 1 - j;
                matrix[i][j] = copy[k][i];
            }
        }
    }
}
```

- 위와 같은 풀이가 가장 직관적이나 저렇게 풀이하지 말라는 뜻이겠죠!
- 90도 회전한 배열은 두 번의 스왑으로 만들어낼 수가 있습니다.

![](https://blog.kakaocdn.net/dn/YmpuM/btsB7nZyupG/5xUvK0KHAJAZIJ6dnKMAFk/img.gif)

- 먼저 **전치행렬(transpose)** 을 만들고 양 끝 컬럼 교환을 해주면 되는 것인데요!
- 전치행렬이란 어떠한 행렬의 열과 행을 바꾸어낸 행렬을 의미합니다.
- 예를 들어서 \[1, 2, 3\], \[4, 5, 6\] 을 전치하면, \[1, 4\], \[2, 5\], \[3, 6\] 으로 3 x 2 행렬이 2 x 3 이 되는 것이죠!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F0A5ui%2FbtsB7gTqsSV%2FiBccDMyv3QNkoGKVKjosQK%2Fimg.png)

- 만약 문제에서처럼 행과 열이 같다면 위와 같이 대각선을 기준으로 스왑이 이루어집니다.

### 🔍 정답

```java
class Solution {
    public void rotate(int[][] matrix) {
        for (int i = 0; i < matrix.length-1; i++) {
            for (int j = i+1; j < matrix.length; j++) {
                transpose(matrix, i, j);
            }
        }

        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix.length/2; j++) {
                reverse(matrix, i, j, matrix.length-1-j);
            }
        }
    }

    public void transpose(int[][] matrix, int a, int b) {
        int temp = matrix[a][b];
        matrix[a][b] = matrix[b][a];
        matrix[b][a] = temp;
    }

    public void reverse(int[][] matrix, int i, int a, int b) {
        int temp = matrix[i][a];
        matrix[i][a] = matrix[i][b];
        matrix[i][b] = temp;
    }
}
```