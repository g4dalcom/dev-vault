# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/154538)

## 📝 문제

자연수 `x`를 `y`로 변환하려고 합니다. 사용할 수 있는 연산은 다음과 같습니다.

-   `x`에 `n`을 더합니다
-   `x`에 2를 곱합니다.
-   `x`에 3을 곱합니다.

자연수 `x`, `y`, `n`이 매개변수로 주어질 때, `x`를 `y`로 변환하기 위해 필요한 최소 연산 횟수를 return하도록 solution 함수를 완성해주세요. 이때 `x`를 `y`로 만들 수 없다면 -1을 return 해주세요.

---

##### 제한사항

-   1 ≤ `x` ≤ `y` ≤ 1,000,000
-   1 ≤ `n` < `y`

---

##### 입출력 예

| x   | y   | n   | result |
|:--- |:--- |:--- |:------ |
| 10  | 40  | 5   | 2      |
| 10  | 40  | 30  | 1      |
| 2   | 5   | 4   | -1       |

---

##### 입출력 예 설명

입출력 예 #1  
`x`에 2를 2번 곱하면 40이 되고 이때가 최소 횟수입니다.

입출력 예 #2  
`x`에 `n`인 30을 1번 더하면 40이 되고 이때가 최소 횟수입니다.

입출력 예 #3  
`x`를 `y`로 변환할 수 없기 때문에 -1을 return합니다.

---

### 🔍 정답(DP)

```java
import java.util.Arrays;

class Solution {
    public int solution(int x, int y, int n) {
        int[] dp = new int[y + 1];

        Arrays.fill(dp, Integer.MAX_VALUE);
        dp[x] = 0;

        for(int i = x; i <= y - n; i++) {
            if(dp[i] == Integer.MAX_VALUE) {
                continue;
            }

            if(i + n <= y) {
                dp[i + n] = Math.min(dp[i + n], dp[i] + 1);
            }
            if(i * 2 <= y) {
                dp[i * 2] = Math.min(dp[i * 2], dp[i] + 1);
            }
            if(i * 3 <= y) {
                dp[i * 3] = Math.min(dp[i * 3], dp[i] + 1);
            }
        }
        return dp[y] == Integer.MAX_VALUE ? -1 : dp[y];
    }
}
```
-  dp배열은 해당 수가 몇 번의 연산으로 도달이 가능한지를 담는다.
	- dp[10] = 1 이라면, 10이라는 숫자는 x에서 한 번의 연산으로 도달이 가능하다는 의미!
- 만약 dp[i] 가 계속 초기화값으로 남아있다면 x에서 도달할 수 없는 값인 것이고
- 도달이 가능하다면 dp[i]에서 한 번의 연산을 더 한 것이므로 dp[i] + 1과 기존에 입력되어있는 값 중 더 작은 값을 가지게 된다.
	- MAX_VALUE가 초기값이기 때문에 dp[i] + 1이 기본적으로 저장이 되나, 전에 이미 도달해서 dp값이 입력되었다면 그 값과 비교해서 더 작은 값을 입력하게 된다.

### 🔎 정답(bfs)

```java
import java.util.*;

class Solution {
    
    static Queue<Integer> q = new LinkedList<>();
    static int[] count = new int[1000001];
    
    public int solution(int x, int y, int n) {
        return bfs(x, y, n);
    }
    
    public static int bfs(int current, int y, int n) {
        q.add(current);
        
        while (!q.isEmpty()) {
            int temp = q.poll();
            
            if (temp == y) {
                return count[y];
            }
            
            addQueue(y, temp, temp + n);
            addQueue(y, temp, temp * 2);
            addQueue(y, temp, temp * 3);
        }
        return -1; 
    }
    
    public static void addQueue(int y, int value, int nextValue) {
        if (nextValue <= y && count[nextValue] == 0) {
            q.add(nextValue);
            count[nextValue] = count[value] + 1;
        }
    }
}
```
- 도달할 수 있는 수를 Queue에 넣으면서 이전 값 +1을 해주는 방법이다.
- Queue에 들어가지 않는 수는 도달할 수 없는 수이므로 -1을 리턴하도록 하였다.