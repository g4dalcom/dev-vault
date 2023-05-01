# [문제링크](https://www.acmicpc.net/problem/10974)

## 📝 문제

N이 주어졌을 때, 1부터 N까지의 수로 이루어진 순열을 사전순으로 출력하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N(1 ≤ N ≤ 8)이 주어진다. 

## 출력

첫째 줄부터 N!개의 줄에 걸쳐서 모든 순열을 사전순으로 출력한다.

## 예제 입력 1 

3

## 예제 출력 1

1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    static int N;
    static int[] arr;
    static StringBuilder sb;
    static boolean[] visit;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());

        arr = new int[N];
        sb = new StringBuilder();
        visit = new boolean[N+1];   // 중복 순열이 아닌, 순열이므로 탐색 여부 체크할 배열 선언

        backtracking(0);
        System.out.println(sb);
    }

    public static void backtracking(int depth) {
        if (depth == N) {
            for (int e : arr) {
                sb.append(e).append(" ");
            }
            sb.append("\n");
        }

        /** 
         * 순열이므로 1 2, 2 1 과 같은 경우를 모두 탐색하기 위해 i = 1부터 탐색하는 것으로 한다!
         * 만약 조합이었다면 메소드의 인자로 탐색을 시작할 인덱스(start)를 받아서 i = start부터 탐색해서
         * 현재 인덱스보다 큰 수들만을 조합하는 방식으로 했을 것이다!
         */
        for (int i = 1; i <= N; i++) {
            if (!visit[i]) {
                visit[i] = true;
                arr[depth] = i;
                backtracking(depth+1);
                visit[i] = false;
            }
        }
    }
}
```
- 기본 순열 문제!