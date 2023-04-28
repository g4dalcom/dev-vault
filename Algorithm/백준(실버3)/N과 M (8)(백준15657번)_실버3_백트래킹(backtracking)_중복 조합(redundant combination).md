# [문제링크](https://www.acmicpc.net/problem/15657)

## 📝 문제

N개의 자연수와 자연수 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오. N개의 자연수는 모두 다른 수이다.

-   N개의 자연수 중에서 M개를 고른 수열
-   같은 수를 여러 번 골라도 된다.
-   고른 수열은 비내림차순이어야 한다.
    -   길이가 K인 수열 A가 A1 ≤ A2 ≤ ... ≤ AK-1 ≤ AK를 만족하면, 비내림차순이라고 한다.

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

1 1
1 7
1 8
1 9
7 7
7 8
7 9
8 8
8 9
9 9

## 예제 입력 3 

4 4
1231 1232 1233 1234

## 예제 출력 3 

1231 1231 1231 1231
1231 1231 1231 1232
1231 1231 1231 1233
1231 1231 1231 1234
1231 1231 1232 1232
1231 1231 1232 1233
1231 1231 1232 1234
1231 1231 1233 1233
1231 1231 1233 1234
1231 1231 1234 1234
1231 1232 1232 1232
1231 1232 1232 1233
1231 1232 1232 1234
1231 1232 1233 1233
1231 1232 1233 1234
1231 1232 1234 1234
1231 1233 1233 1233
1231 1233 1233 1234
1231 1233 1234 1234
1231 1234 1234 1234
1232 1232 1232 1232
1232 1232 1232 1233
1232 1232 1232 1234
1232 1232 1233 1233
1232 1232 1233 1234
1232 1232 1234 1234
1232 1233 1233 1233
1232 1233 1233 1234
1232 1233 1234 1234
1232 1234 1234 1234
1233 1233 1233 1233
1233 1233 1233 1234
1233 1233 1234 1234
1233 1234 1234 1234
1234 1234 1234 1234

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;
import java.util.*;

public class Main {
    static int N, M;
    static int[] arr;
    static int[] result;

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

        recombination(0, 0);
    }

    public static void recombination(int idx, int depth) {
        
        // result 배열이 다 차면 출력해주기
        if (depth == M) {
            for (int i : result) {
                System.out.print(i + " ");
            }
            System.out.println();
            return;
        }

        // 중복 조합이므로 방문 체크 배열이 없고 i = idx 부터 시작해서 자신보다 작은 수는 고려하지 않음!
        for (int i = idx; i < N; i++) {
            result[depth] = arr[i];
            recombination(i, depth + 1);
        }
    }
}
```
- 백트래킹의 기초 중복 조합 문제이다!
- 중복 조합에서 중요한 점은 
	- 중복을 고려하기 때문에 방문 체크 배열이 없다는 것
	- 순서가 없어서 (1, 2)와 (2, 1) 같은 경우를 같게 보므로 자신보다 작은 수는 고려하지 않는다는 것!