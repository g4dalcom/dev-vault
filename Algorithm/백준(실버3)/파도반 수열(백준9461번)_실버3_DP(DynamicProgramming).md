# [문제링크](https://www.acmicpc.net/problem/9461)

## 📝 문제

오른쪽 그림과 같이 삼각형이 나선 모양으로 놓여져 있다. 첫 삼각형은 정삼각형으로 변의 길이는 1이다. 그 다음에는 다음과 같은 과정으로 정삼각형을 계속 추가한다. 나선에서 가장 긴 변의 길이를 k라 했을 때, 그 변에 길이가 k인 정삼각형을 추가한다.

![](https://www.acmicpc.net/upload/images/pandovan.png)

파도반 수열 P(N)은 나선에 있는 정삼각형의 변의 길이이다. P(1)부터 P(10)까지 첫 10개 숫자는 1, 1, 1, 2, 2, 3, 4, 5, 7, 9이다.

N이 주어졌을 때, P(N)을 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 테스트 케이스의 개수 T가 주어진다. 각 테스트 케이스는 한 줄로 이루어져 있고, N이 주어진다. (1 ≤ N ≤ 100)

## 출력

각 테스트 케이스마다 P(N)을 출력한다.

## 예제 입력 1 

2
6
12

## 예제 출력 1 

3
16


---

### 🔍 정답

```java
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
  
public class Main {  
  
    public static void main(String[] args) throws IOException {  
  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        int T = Integer.parseInt(br.readLine());  
  
        StringBuilder sb = new StringBuilder();  
        long[] dp = new long[101];  
        dp[1] = 1;  
        dp[2] = 1;  
        dp[3] = 1;  
        dp[4] = 2;  
        dp[5] = 2;  
        for (int i = 6; i < 101; i++) {  
            dp[i] = dp[i-1] + dp[i-5];  
        }  
  
        for (int i = 0; i < T; i++) {  
            int n = Integer.parseInt(br.readLine());  
            sb.append(dp[n]).append("\n");  
        }  
        System.out.println(sb);  
    }  
}
```
- 규칙을 찾다가 처음에 만들어본 점화식

```java
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
  
public class Main {  
  
    public static void main(String[] args) throws IOException {  
  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        int T = Integer.parseInt(br.readLine());  
  
        StringBuilder sb = new StringBuilder();  
        long[] dp = new long[101];  
        dp[1] = 1;  
        dp[2] = 1;  
        dp[3] = 1;  
        for (int i = 4; i < 101; i++) {  
            dp[i] = dp[i-2] + dp[i-3];  
        }  
  
        for (int i = 0; i < T; i++) {  
            int n = Integer.parseInt(br.readLine());  
            sb.append(dp[n]).append("\n");  
        }  
        System.out.println(sb);  
    }  
}
```
- 피보나치는 현재 항의 값은 앞의 두 항의 값의 합으로 이루어진다.
	- 1 1 2 3 5 8 . . .
- 문제는 피보나치 수열 형식이긴 하지만 앞의 두 항의 합이 현재 항 다음 항에 나타난다.
	- 1 1 1 2 2 3 4 5 7 . . .
- 따라서 점화식을 dp[i] = dp[i-2] + dp[i-3] 으로 세워주면 되고 dp[100]의 경우 9,000억 가까이 되므로 long타입 배열을 선언해서 담아주었다.