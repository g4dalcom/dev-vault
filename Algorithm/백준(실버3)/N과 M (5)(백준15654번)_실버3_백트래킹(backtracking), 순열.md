# [문제링크](https://www.acmicpc.net/problem/15654)

## 📝 문제

N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오. N개의 자연수는 모두 다른 수이다.

-   N개의 자연수 중에서 M개를 고른 수열

## 입력

첫째 줄에 N과 M이 주어진다. (1 ≤ M ≤ N ≤ 8)

둘째 줄에 N개의 수가 주어진다. 입력으로 주어지는 수는 10,000보다 작거나 같은 자연수이다.

## 출력

한 줄에 하나씩 문제의 조건을 만족하는 수열을 출력한다. 중복되는 수열을 여러 번 출력하면 안되며, 각 수열은 공백으로 구분해서 출력해야 한다.

수열은 사전 순으로 증가하는 순서로 출력해야 한다.

## 예제 입력 1 

3 1
4 5 2

## 예제 출력 1 

2
4
5

## 예제 입력 2 

4 2
9 8 7 1

## 예제 출력 2 

1 7
1 8
1 9
7 1
7 8
7 9
8 1
8 7
8 9
9 1
9 7
9 8

## 예제 입력 3 

4 4
1231 1232 1233 1234

## 예제 출력 3 

1231 1232 1233 1234
1231 1232 1234 1233
1231 1233 1232 1234
1231 1233 1234 1232
1231 1234 1232 1233
1231 1234 1233 1232
1232 1231 1233 1234
1232 1231 1234 1233
1232 1233 1231 1234
1232 1233 1234 1231
1232 1234 1231 1233
1232 1234 1233 1231
1233 1231 1232 1234
1233 1231 1234 1232
1233 1232 1231 1234
1233 1232 1234 1231
1233 1234 1231 1232
1233 1234 1232 1231
1234 1231 1232 1233
1234 1231 1233 1232
1234 1232 1231 1233
1234 1232 1233 1231
1234 1233 1231 1232
1234 1233 1232 1231

---

### 🔍 정답

```java
import java.io.IOException;
import java.util.*;
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main {
    static int N, M;
    static int[] arr;
    static int[] result;
    static boolean[] visit;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        st = new StringTokenizer(br.readLine());
        arr = new int[N];
        for (int i = 0; i < N; i++) {
            arr[i] = Integer.parseInt(st.nextToken());
        }

        Arrays.sort(arr);

        result = new int[M];
        visit = new boolean[N];

        backtracking(0);
    }

    public static void backtracking(int depth) {
        if (depth == M) {
            for (int i : result) {
                System.out.print(i + " ");
            }
            System.out.println();
            return;
        }

        // 순서가 있는 순열이므로 i = 0부터 시작해서 1 2와 2 1 과 같은 형태를 모두 포함한다!
        for (int i = 0; i < arr.length; i++) {
            if (!visit[i]) {
                visit[i] = true;
                result[depth] = arr[i];
                backtracking(depth + 1);
                visit[i] = false;
            }
        }
    }
}
```