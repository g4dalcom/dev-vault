# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12952?language=java)

## 📝 문제

가로, 세로 길이가 n인 정사각형으로된 체스판이 있습니다. 체스판 위의 n개의 퀸이 서로를 공격할 수 없도록 배치하고 싶습니다.

예를 들어서 n이 4인경우 다음과 같이 퀸을 배치하면 n개의 퀸은 서로를 한번에 공격 할 수 없습니다.

![Imgur](https://i.imgur.com/lt2zdK6.png)  
![Imgur](https://i.imgur.com/5c5EUrq.png)

체스판의 가로 세로의 세로의 길이 n이 매개변수로 주어질 때, n개의 퀸이 조건에 만족 하도록 배치할 수 있는 방법의 수를 return하는 solution함수를 완성해주세요.

##### 제한사항

- 퀸(Queen)은 가로, 세로, 대각선으로 이동할 수 있습니다.
- n은 12이하의 자연수 입니다.

---

##### 입출력 예

|n|result|
|---|---|
|4|2|

---

### 💡 풀이

[[N-Queen(백준9663번)_골드4_백트래킹]]


### 🔍 정답

```java
class Solution {
    
    static int count = 0;
    
    public int solution(int n) {
		// 인덱스는 열, 값은 행!
        int[] board = new int[n];
        
        dfs(n, 0, board);
        
        return count;
    }
    
    public void dfs(int n, int depth, int[] board) {
        if (n == depth) {
            count++;
            return;
        }
        
        for (int i = 0; i < n; i++) {
            board[depth] = i;
            if (possible(depth, board)) {
                dfs(n, depth+1, board);
            }
        }
    }
    
    public boolean possible(int x, int[] board) {

		/**
         * 행이 같은 경우와 (board[x] == board[i])
         * 대각선 위치인 경우 (각 행의 차이와 열의 차이가 같을 때) false를 반환한다.
         */
        for (int i = 0; i < x; i++) {
            if (board[i] == board[x] || Math.abs(board[i] - board[x]) == Math.abs(i - x)) {
                return false;
            }
        }
        return true;
    }
}
```