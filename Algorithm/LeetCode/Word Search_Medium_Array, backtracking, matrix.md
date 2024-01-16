# [문제링크](https://leetcode.com/problems/word-search)

## 📝 문제

Given an `m x n` grid of characters `board` and a string `word`, return `true` _if_ `word` _exists in the grid_.

The word can be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/04/word2.jpg)

**Input:** board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/04/word-1.jpg)

**Input:** board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "SEE"
**Output:** true

**Example 3:**

![](https://assets.leetcode.com/uploads/2020/10/15/word3.jpg)

**Input:** board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCB"
**Output:** false

**Constraints:**

- `m == board.length`
- `n = board[i].length`
- `1 <= m, n <= 6`
- `1 <= word.length <= 15`
- `board` and `word` consists of only lowercase and uppercase English letters.

**Follow up:** Could you use search pruning to make your solution faster with a larger `board`?


---

### 💡 풀이

- 상하좌우 네 방향으로 좌표 이동이 가능한 board 안에서, 주어진 word를 만들 수 있는지 여부를 판단해야 하는 문제입니다.
- word의 인덱스를 depth로 두고 가능한 경우에 depth를 늘려가며 다음 인덱스가 현재 위치에서 이동 가능한 좌표에 존재하는지를 확인하며 탐색하여 구현하였습니다.

### 🔍 정답

```java
class Solution {
    char[][] board;
    String word;
    boolean[][] visit;
    int[] dx = {0, 0, 1, -1};
    int[] dy = {1, -1, 0, 0};
    boolean isWordExist;

    public boolean exist(char[][] board, String word) {
        this.board = board;
        this.word = word;
        isWordExist = false;

        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (board[i][j] == word.charAt(0)) {
                    visit = new boolean[board.length][board[0].length];
                    visit[i][j] = true;
                    dfs(i, j, 1);
                }
            }
        }

        return isWordExist;
    }
    
    public void dfs(int row, int col, int depth) {
        if (depth == word.length()) {
            isWordExist = true;
            return;
        }

        if (!isWordExist) {
            for (int i = 0; i < 4; i++) {
                int cx = row + dx[i];
                int cy = col + dy[i];

                if (cx >= 0 && cy >= 0 && cx < board.length && cy < board[0].length) {
                    if (!visit[cx][cy] && board[cx][cy] == word.charAt(depth)) {
                        visit[cx][cy] = true;
                        dfs(cx, cy, depth+1);
                        visit[cx][cy] = false;
                    }
                }
            }
        }
    }
}
```