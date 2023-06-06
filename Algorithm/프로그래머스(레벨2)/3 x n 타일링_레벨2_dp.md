# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12902)

## 📝 문제

###### 문제 설명

가로 길이가 2이고 세로의 길이가 1인 직사각형 모양의 타일이 있습니다. 이 직사각형 타일을 이용하여 세로의 길이가 3이고 가로의 길이가 n인 바닥을 가득 채우려고 합니다. 타일을 채울 때는 다음과 같이 2가지 방법이 있습니다

- 타일을 가로로 배치 하는 경우
- 타일을 세로로 배치 하는 경우

예를들어서 n이 8인 직사각형은 다음과 같이 채울 수 있습니다.

![Imgur](https://i.imgur.com/zBW7peI.png)

직사각형의 가로의 길이 n이 매개변수로 주어질 때, 이 직사각형을 채우는 방법의 수를 return 하는 solution 함수를 완성해주세요.

##### 제한사항

- 가로의 길이 n은 5,000이하의 자연수 입니다.
- 경우의 수가 많아 질 수 있으므로, 경우의 수를 1,000,000,007으로 나눈 나머지를 return해주세요.

##### 입출력 예

|n|result|
|---|---|
|4|11|

##### 입출력 예 설명

입출력 예 #1  
다음과 같이 11가지 방법이 있다.  
![Imgur](https://i.imgur.com/nnoT9kL.png)  
![Imgur](https://i.imgur.com/QTZFrTH.png)  
![Imgur](https://i.imgur.com/YE1JfJn.png)  
![Imgur](https://i.imgur.com/QhYvRTr.png)  
![Imgur](https://i.imgur.com/NKgKTIR.png)  
![Imgur](https://i.imgur.com/3uobFxe.png)  
![Imgur](https://i.imgur.com/sEK9oor.png)  
![Imgur](https://i.imgur.com/u6dpiep.png)  
![Imgur](https://i.imgur.com/re3C19N.png)  
![Imgur](https://i.imgur.com/GerdAJB.png)  
![Imgur](https://i.imgur.com/ITcbWj0.png)

---

### 💡 풀이

- [참고 사이트](https://pangtrue.tistory.com/310)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FD4TC2%2Fbtr3lC2McoJ%2Fi9g074d3nqvi68JP0Zzln0%2Fimg.png)
- 우선 주어진 타일 2 X 1, 1 X 2로 짝수이므로 면적 또한 짝수여야 하므로 홀수인 경우는 제외하고 생각해야 한다.
- 가장 기본적인 3 X 2 타일은 경우의 수가 3개이다.
- 다음 3 X 4 타일은 N[2] X 3으로 구할 수 있을 것 같지만 특수 케이스가 2개 존재한다. 따라서 3 X 3 + 2 = 11개이다.
- 여기까지는 직접 그려보면서 구해볼 수 있지만 N = 6만 되어도 그리면서 구할 수 있는 범위를 넘어간다. 이때부터는 주어진 조건을 가지고 규칙을 찾아야 할 때이다.
- N[6]을 구할 때 간단하게 생각하면 N[4] X 3으로 구할 수 있을 것 같지만 이 또한 특수 케이스가 존재한다.
	- 첫째로, N[6]을 쪼개면 N[4]모양이 두 가지로 나오고 N[2]모양은 3가지로 들어갈 수 있으므로 + N[2] X 2를 해주어야 한다.
	- 둘째로, N[6]의 경우도 N[4]에서 존재하지 않던 특수 케이스가 2개 존재한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F3X8rn%2Fbtr3gLyYPeG%2FNe1t1XZsUQhXIc9QSkkSck%2Fimg.png)

- 따라서 규칙에 따른 점화식을 구해보면,

$$dp[N] = dp[N-2] \times dp[2] + dp[N-4]\times2$$
- 이 때 dp[N-4] X 2로 구할 수 있는 경우는 N값이 커질 경우 더 존재하게 되므로 sum이라는 변수에 dp[N-2]를 누적해서 더해준다.


### 🔍 정답

```java
class Solution {
    public int solution(int n) {
        final int MOD = 1000000007;
          
        if (n % 2 == 1) return 0;
        
        long[] dp = new long[n+1];
        dp[0] = 1;
        
        long sum = 0;
        for (int i = 2; i <= n; i += 2) {
            dp[i] = dp[i-2] * 3 + sum * 2;
            sum += dp[i-2];
            
            dp[i] = dp[i] % MOD;
        }
        
        return (int) dp[n];
    }
}
```