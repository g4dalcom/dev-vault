# [문제링크](https://www.acmicpc.net/problem/9251)

## 📝 문제

LCS(Longest Common Subsequence, 최장 공통 부분 수열)문제는 두 수열이 주어졌을 때, 모두의 부분 수열이 되는 수열 중 가장 긴 것을 찾는 문제이다.

예를 들어, ACAYKP와 CAPCAK의 LCS는 ACAK가 된다.

## 입력

첫째 줄과 둘째 줄에 두 문자열이 주어진다. 문자열은 알파벳 대문자로만 이루어져 있으며, 최대 1000글자로 이루어져 있다.

## 출력

첫째 줄에 입력으로 주어진 두 문자열의 LCS의 길이를 출력한다.

## 예제 입력 1

ACAYKP
CAPCAK

## 예제 출력 1

4

---

### 🔍 정답(Bottom-up)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        char[] charA = br.readLine().toCharArray();
        char[] charB = br.readLine().toCharArray();

        /**
         * dp[기준값][비교대상] 으로 값을 채운다.
         * dp[0][2] 라면, charA[0]번 인덱스(예시, A)와 charB의 0, 1, 2 인덱스(예시, C A P) 를 비교한다.
         * dp값은 0부터 매겨지므로 배열의 길이는 +1
         */
        int[][] dp = new int[charA.length+1][charB.length+1];

        for (int i = 1; i <= charA.length; i++) {
            for (int j = 1; j <= charB.length; j++) {

                /**
                 * 문자가 같을 때 dp[i-1][j-1]로 이전 값을 참조해야 하므로 인덱스는 1부터 시작하고
                 * 대신 i-1, j-1을 해서 0번 문자부터 확인한다.
                 */
                if (charA[i-1] == charB[j-1]) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                }
                else {
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
        System.out.println(dp[charA.length][charB.length]);
    }
}
```
- [참고 사이트](https://st-lab.tistory.com/139)
- 참고 사이트의 설명을 여러번 보면서 이해는 했는데 막상 비슷한 문제가 응용되어 나오면 헤맬 것 같다.
- LCS는 워낙 유명한 문제라고 하니, 관련 문제를 몇 번 더 풀어봐야 될 듯하다.
- 가장 중요한 것은 dp를 2차원 배열로 채운다는 것과, dp값을 어떻게 채워나가느냐, 그리고 비교값이 같을 때와 다를 때 어떤 dp값을 가져와서 채우느냐였다.


### 🔎 정답(Top-down)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    static char[] charA, charB;
    static Integer[][] dp;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        charA = br.readLine().toCharArray();
        charB = br.readLine().toCharArray();

        dp = new Integer[charA.length][charB.length];

        System.out.println(LCS(charA.length-1, charB.length-1));
    }

    public static int LCS(int x, int y) {
        System.out.println("x = " + x);
        System.out.println("y = " + y);

        if (x == -1 || y == -1) return 0;

        if (dp[x][y] == null) {
            dp[x][y] = 0;

            if (charA[x] == charB[y]) {
                dp[x][y] = LCS(x-1, y-1) + 1;
            }
            else {
                dp[x][y] = Math.max(LCS(x-1, y), LCS(x, y-1));
            }
        }
        return dp[x][y];
    }
}
```
- 재귀로 풀어본 방식이다. 점화식만 알면 구현은 더 간단하지만 성능면에서는 반복문이 낫다고 한다.