# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12905#qna)

## 📝 문제

###### 문제 설명

1와 0로 채워진 표(board)가 있습니다. 표 1칸은 1 x 1 의 정사각형으로 이루어져 있습니다. 표에서 1로 이루어진 가장 큰 정사각형을 찾아 넓이를 return 하는 solution 함수를 완성해 주세요. (단, 정사각형이란 축에 평행한 정사각형을 말합니다.)

예를 들어

|1|2|3|4|
|---|---|---|---|
|0|1|1|1|
|1|1|1|1|
|1|1|1|1|
|0|0|1|0|

가 있다면 가장 큰 정사각형은

|1|2|3|4|
|---|---|---|---|
|0|`1`|`1`|`1`|
|1|`1`|`1`|`1`|
|1|`1`|`1`|`1`|
|0|0|1|0|

가 되며 넓이는 9가 되므로 9를 반환해 주면 됩니다.

##### 제한사항

- 표(board)는 2차원 배열로 주어집니다.
- 표(board)의 행(row)의 크기 : 1,000 이하의 자연수
- 표(board)의 열(column)의 크기 : 1,000 이하의 자연수
- 표(board)의 값은 1또는 0으로만 이루어져 있습니다.

---

##### 입출력 예

|board|answer|
|---|---|
|[[0,1,1,1],[1,1,1,1],[1,1,1,1],[0,0,1,0]]|9|
|[[0,0,1,1],[1,1,1,1]]|4|

##### 입출력 예 설명

입출력 예 #1  
위의 예시와 같습니다.

입출력 예 #2  
| 0 | 0 | `1` | `1` |  
| 1 | 1 | `1` | `1` |  
로 가장 큰 정사각형의 넓이는 4가 되므로 4를 return합니다.

---

### 💡 풀이

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FWgsJZ%2FbtsiOK5GgKN%2Fe3l5yiaHZTTF44E76uh25K%2Fimg.png)

현재 위치 기준으로 위, 왼쪽, 왼쪽위 세군데를 탐색하는데 그러기 위해서는 우선 시작 위치가 (1, 1)이어야 한다.

탐색하는 세 곳 중 0이 존재한다면 2 x 2 정사각형은 만들어질 수 없으므로 정사각형은 최대 1 x 1 이 된다.

만약 탐색하는 세 곳이 모두 1이라면 2 x 2 정사각형이 만들어진 경우이므로 현재 위치를 2로 갱신한다.

위와 같은 논리로 탐색한 세 곳 중 가장 작은 값 + 1이 현재 위치라는 것을 알 수 있다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbjphCO%2FbtsiPjmsfTo%2FGkICVOJzSn6XHBHasF8alK%2Fimg.png)

모두 탐색하고나면 위와 같은 그림이 그려진다.

가장 큰 값이 3이므로 3 x 3 = 9 의 정사각형이 최댓값인 것!


### 🔍 정답

```java
class Solution {    
    public int solution(int [][]board) {
		// row나 col이 1 이하라면 최대 만들어질 수 있는 정사각형의 크기는 1이 된다.
        if (board.length <= 1 || board[0].length <= 1) {
            for (int i = 0; i < board.length; i++) {
                for (int j = 0; j < board[i].length; j++) {
                    if (board[i][j] == 1) return 1;
                }
            }
            return 0;
        }
        
        int max = Integer.MIN_VALUE;
        
        for (int i = 1; i < board.length; i++) {
            for (int j = 1; j < board[0].length; j++) {
                if (board[i][j] >= 1) {
                    int up = board[i-1][j];
                    int left = board[i][j-1];
                    int upLeft = board[i-1][j-1];
                    
                    board[i][j] = Math.min(Math.min(up, left), upLeft) + 1;
                    max = Math.max(max, board[i][j]);
                }
            }
        }
        
        return max * max;
    }
}
```