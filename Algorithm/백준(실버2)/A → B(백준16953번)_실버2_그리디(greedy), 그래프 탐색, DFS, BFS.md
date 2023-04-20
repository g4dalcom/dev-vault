# [문제링크](https://www.acmicpc.net/problem/16953)

## 📝 문제

정수 A를 B로 바꾸려고 한다. 가능한 연산은 다음과 같은 두 가지이다.

-   2를 곱한다.
-   1을 수의 가장 오른쪽에 추가한다. 

A를 B로 바꾸는데 필요한 연산의 최솟값을 구해보자.

## 입력

첫째 줄에 A, B (1 ≤ A < B ≤ 109)가 주어진다.

## 출력

A를 B로 바꾸는데 필요한 연산의 최솟값에 1을 더한 값을 출력한다. 만들 수 없는 경우에는 -1을 출력한다.

## 예제 입력 1

2 162

## 예제 출력 1

5

2 → 4 → 8 → 81 → 162

## 예제 입력 2 

4 42

## 예제 출력 2 

-1

## 예제 입력 3 

100 40021

## 예제 출력 3 

5

---

### ❌ 오답 (DP)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    static int A, B;
    static int[] dp;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        A = Integer.parseInt(st.nextToken());
        B = Integer.parseInt(st.nextToken());

        dp = new int[B+1];
        dp[A] = 1;

        dfs(A);
        System.out.println(dp[B] == 0 ? -1 : dp[B]);
    }

    public static void dfs(int num) {

        if (num * 2 <= B && dp[num * 2] == 0) {
            dp[num * 2] = dp[num] + 1;
            dfs(num * 2);
        }
        if (num * 10 + 1 <= B && dp[num * 10 + 1] == 0) {
            dp[num * 10 + 1] = dp[num] + 1;
            dfs(num * 10 + 1);
        }
    }
}
```
- 처음에는 DP + DFS 로 풀어보려고 했는데 메모리 초과 발생..
- 아마도 dp배열의 크기가 매우 커져서가 아닐까 예상하고 다른 풀이를 생각해보았다.


### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int A = Integer.parseInt(st.nextToken());
        int B = Integer.parseInt(st.nextToken());

        int cnt = 1;
        while (B > A) {
            if (B % 10 == 1) {
                B = B / 10;
            } else {
                if ((double) B / 2 == Math.ceil((double) B / 2)) B = B / 2;
                else break;
            }
            cnt++;
        }

        System.out.println(B == A ? cnt : -1);
    }
}
```
- A -> B를 만드는 것과 B -> A 를 만드는 것은 방향의 차이만 있고 연산의 최솟값을 구하는 데는 동일하다고 생각하였다.
- 그리고 B -> A 방향으로 진행할 때 확실한 규칙이 보였다.
- B의 일의 자리가 1이라면 1을 제거하는 연산을 하고 그 외에는 2를 나누는 연산을 하는 것!
- 여기서 나눗셈은 소숫점이 발생할 수가 있는데 그래서 발생하는 오류를 방지하기 위해 B / 2 와 B / 2를 올림한 값이 같을 경우(소숫점이 없는 경우)에만 연산을 계속하도록 하였다!

### 🔎 정답(DFS)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    static long A, B;
    static boolean check;
    static int result = 0;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        A = Long.parseLong(st.nextToken());
        B = Long.parseLong(st.nextToken());

        dfs(A, 1);

        System.out.println(check ? result : -1);
    }

    public static void dfs(long num, int cnt) {
        if (num == B) {
            result = cnt;
            check = true;
            return;
        }

        if (num * 2 <= B) {
            dfs(num * 2, cnt+1);
        }
        if (num * 10 + 1 <= B) {
            dfs(num * 10 + 1, cnt+1);
        }
    }
}
```
- 정답을 제출하고나서 다른 사람들의 풀이를 찾아보았는데 그리디 알고리즘임에도 DFS나 BFS를 이용해서 해결한 사람들이 많았다.
- 그래서 나도 DP를 빼고 DFS로만 구현을 해보았는데 위의 정답과 메모리와 시간이 거의 비슷했다!