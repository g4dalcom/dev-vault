# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/43105)

## 📝 문제

###### 문제 설명

![스크린샷 2018-09-14 오후 5.44.19.png](https://grepp-programmers.s3.amazonaws.com/files/production/97ec02cc39/296a0863-a418-431d-9e8c-e57f7a9722ac.png)

위와 같은 삼각형의 꼭대기에서 바닥까지 이어지는 경로 중, 거쳐간 숫자의 합이 가장 큰 경우를 찾아보려고 합니다. 아래 칸으로 이동할 때는 대각선 방향으로 한 칸 오른쪽 또는 왼쪽으로만 이동 가능합니다. 예를 들어 3에서는 그 아래칸의 8 또는 1로만 이동이 가능합니다.

삼각형의 정보가 담긴 배열 triangle이 매개변수로 주어질 때, 거쳐간 숫자의 최댓값을 return 하도록 solution 함수를 완성하세요.

##### 제한사항

- 삼각형의 높이는 1 이상 500 이하입니다.
- 삼각형을 이루고 있는 숫자는 0 이상 9,999 이하의 정수입니다.

##### 입출력 예

|triangle|result|
|---|---|
|[[7], [3, 8], [8, 1, 0], [2, 7, 4, 4], [4, 5, 2, 6, 5]]|30|

---

### 💡 풀이

- 삼각형의 모서리쪽에 있는 수는 바로 위의 수가 그대로 내려오게 됩니다. 인덱스를 반영해서 작성해주면 아래와 같이 나옵니다.

```java
triangle[i][0] += triangle[i-1][0];
triangle[i][i] += triangle[i-1][i-1];
```

- 왼쪽 모서리는 항상 column 값이 0이니까 row-1의 0번 column을 내려주면 되고 오른쪽 모서리는 인덱스상 \[row\]\[row\] 이므로 \[row-1]\[row-1] 을 내려주면 되는 것이죠!
- 그 외 다른 곳들은 자신의 왼쪽 위와 오른쪽 위에 누적된 값중 더 큰 값을 가져오면 마지막 row에서는 내려올 수 있는 가장 큰 값이 누적되게 됩니다.
- 저는 dp배열을 따로 생성해서 풀이했는데 원본 배열에 그대로 값을 누적하는 것도 나쁘지 않아보이네요!

### 🔍 정답

```java
class Solution {
    public int solution(int[][] triangle) {
        int max_result = 0;
        int[][] dp = new int[triangle.length][triangle.length];
        
        for (int i = 0; i < triangle.length; i++) {
            for (int j = 0; j < triangle[i].length; j++) {
                int current = triangle[i][j];
                if (i == 0) {
                    dp[i][j] = current;
                } else if (j == 0) {
                    dp[i][j] = dp[i-1][j] + current;
                } else if (i == j) {
                    dp[i][j] = dp[i-1][j-1] + current;
                } else {
                    dp[i][j] = Math.max(dp[i-1][j-1] + current, dp[i-1][j] + current);
                }
                
                max_result = Math.max(max_result, dp[i][j]);
            }
        }
        
        return max_result;
    }
}
```


### ✔ 개선된 정답

```java
import java.util.*;

class Solution {
    public int solution(int[][] triangle) {
        for (int i = 1; i < triangle.length; i++) {
            triangle[i][0] += triangle[i-1][0];
            triangle[i][i] += triangle[i-1][i-1];
            
            for (int j = 1; j < i; j++) {
                triangle[i][j] += Math.max(triangle[i-1][j-1], triangle[i-1][j]);
            }
        }
        
        return Arrays.stream(triangle[triangle.length-1]).max().getAsInt();
    }
}
```