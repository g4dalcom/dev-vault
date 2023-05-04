# [문제링크](https://www.acmicpc.net/problem/1920)

## 📝 문제

N개의 정수 A[1], A[2], …, A[N]이 주어져 있을 때, 이 안에 X라는 정수가 존재하는지 알아내는 프로그램을 작성하시오.

## 입력

첫째 줄에 자연수 N(1 ≤ N ≤ 100,000)이 주어진다. 다음 줄에는 N개의 정수 A[1], A[2], …, A[N]이 주어진다. 다음 줄에는 M(1 ≤ M ≤ 100,000)이 주어진다. 다음 줄에는 M개의 수들이 주어지는데, 이 수들이 A안에 존재하는지 알아내면 된다. 모든 정수의 범위는 -231 보다 크거나 같고 231보다 작다.

## 출력

M개의 줄에 답을 출력한다. 존재하면 1을, 존재하지 않으면 0을 출력한다.

## 예제 입력 1 

5
4 1 5 2 3
5
1 3 7 9 5

## 예제 출력 1 

1
1
0
0
1

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;

        int N = Integer.parseInt(br.readLine());
        st = new StringTokenizer(br.readLine());
        int[] origin = new int[N];

        for (int i = 0; i < N; i++) {
            origin[i] = Integer.parseInt(st.nextToken());
        }

        Arrays.sort(origin);    // 이분 탐색을 위해서 원본 배열 정렬!

        int M = Integer.parseInt(br.readLine());
        st = new StringTokenizer(br.readLine());
        int[] check = new int[M];

        // 입력 받으면서 체킹까지!
        for (int i = 0; i < M; i++) {
            check[i] = Integer.parseInt(st.nextToken());

            System.out.println(checkNum(check[i], origin) ? 1 : 0);
        }

    }

    public static boolean checkNum(int num, int[] origin) {
        int lo = 0;
        int hi = origin.length - 1;

        // 이분 탐색!
        while (lo <= hi) {
            int mid = (lo + hi) / 2;

            if (origin[mid] == num) {
                return true;
            }

            if (origin[mid] < num) {
                lo = mid + 1;
            } else {
                hi = mid - 1;
            }
        }

        return false;
    }
}
```
- 기본적인 이분 탐색 문제이다!