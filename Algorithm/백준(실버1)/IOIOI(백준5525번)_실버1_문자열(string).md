# [문제링크](https://www.acmicpc.net/problem/5525)

## 📝 문제

+1개의 `I`와 N개의 `O`로 이루어져 있으면, `I`와 `O`이 교대로 나오는 문자열을 PN이라고 한다.

-   P1 `IOI`
-   P2 `IOIOI`
-   P3 `IOIOIOI`
-   PN `IOIOI...OI` (`O`가 N개)

`I`와 `O`로만 이루어진 문자열 S와 정수 N이 주어졌을 때, S안에 PN이 몇 군데 포함되어 있는지 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N이 주어진다. 둘째 줄에는 S의 길이 M이 주어지며, 셋째 줄에 S가 주어진다.

## 출력

S에 PN이 몇 군데 포함되어 있는지 출력한다.

## 제한

-   1 ≤ N ≤ 1,000,000
-   2N+1 ≤ M ≤ 1,000,000
-   S는 `I`와 `O`로만 이루어져 있다.

## 서브태스크

| 번호 | 배점 | 제한                  |
|:---- |:---- |:--------------------- |
| 1    | 50   | N <= 100, M <= 10,000 |
| 2    | 50   | 추가적인 제약 조건이 없다.                      |


## 예제 입력 1 

1
13
OOIOIOIOIIOII

## 예제 출력 1 

4

-   `OOIOIOIOIIOII`
-   `OOIOIOIOIIOII`
-   `OOIOIOIOIIOII`
-   `OOIOIOIOIIOII`

## 예제 입력 2

2
13
OOIOIOIOIIOII

## 예제 출력 2 

2

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static void main(String[] args) throws IOException {
       BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
       int N = Integer.parseInt(br.readLine());
       int M = Integer.parseInt(br.readLine());
       char[] S = br.readLine().toCharArray();

       int[] dp = new int[M];

       int cnt = 0;
        /**
         * OI가 반복되면서 맨 앞에 I가 붙는 형식이므로
         * OI가 N개가 될 때까지 세어주고 OI가 N개가 되면 맨 처음 OI의 앞 인덱스가 I인지 확인해서 카운팅!
         */
       for (int i = 1; i < M-1; i++) {
           if (S[i] == 'O' && S[i+1] == 'I') {
               dp[i+1] = dp[i-1] + 1;

               if (dp[i+1] >= N && S[i - 2 * N + 1] == 'I') cnt++;
           }
       }
        System.out.println(cnt);
    }
}
```


### ❌ 오답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static void main(String[] args) throws IOException {
       BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
       int N = Integer.parseInt(br.readLine());
       int M = Integer.parseInt(br.readLine());
       String S = br.readLine();

       String tmp = "";
       for (int i = 0; i < N; i++) {
           tmp += "IO";
       }
       tmp += "I";

       int idx = 0;
       int cnt = 0;
       while (true) {
           if (S.indexOf(tmp, idx) == -1) break;

           else if (S.indexOf(tmp, idx) >= 0) {
               cnt++;
               idx = S.indexOf(tmp, idx)+1;
           }
       }
       System.out.println(cnt);
    }
}
```
- N에 따라 비교의 기준이 되는 String을 만들고(tmp)
- tmp가 전체 배열에서 어떤 인덱스에 존재하는지 확인하고, 있다면 카운팅을 하면서 찾은 인덱스+1 부터 다시 탐색하는 식으로 구해보았다.
- 그런데 이 방법은 서브태스크 2번에서 시간 초과가 난다 ㅠㅠ