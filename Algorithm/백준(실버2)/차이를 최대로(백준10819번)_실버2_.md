# [문제링크](https://www.acmicpc.net/problem/10819)

## 📝 문제

N개의 정수로 이루어진 배열 A가 주어진다. 이때, 배열에 들어있는 정수의 순서를 적절히 바꿔서 다음 식의 최댓값을 구하는 프로그램을 작성하시오.

|A[0] - A[1]| + |A[1] - A[2]| + ... + |A[N-2] - A[N-1]|

## 입력

첫째 줄에 N (3 ≤ N ≤ 8)이 주어진다. 둘째 줄에는 배열 A에 들어있는 정수가 주어진다. 배열에 들어있는 정수는 -100보다 크거나 같고, 100보다 작거나 같다.

## 출력

첫째 줄에 배열에 들어있는 수의 순서를 적절히 바꿔서 얻을 수 있는 식의 최댓값을 출력한다.

## 예제 입력 1 

6
20 1 15 8 4 10

## 예제 출력 1 

62


---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    static int N;
    static int max = 0;
    static int[] arr, newArr;
    static boolean[] visit;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());

        arr = new int[N]; // 입력값을 담을 배열
        newArr = new int[N]; // 요소들을 재조합해서 담을 배열
        visit = new boolean[N]; // 방문 여부를 체크할 배열
        StringTokenizer st = new StringTokenizer(br.readLine());

        /* 입력값을 담는다! */
        for (int i = 0; i < N; i++) {
            arr[i] = Integer.parseInt(st.nextToken());
        }

        dfs(0);
        System.out.println(max);
    }

    public static void dfs(int cnt) {
        /* 배열이 모두 담기면 calculate()로 계산을 하고 계산값을 max와 비교하여 갱신해준다! */
        if (cnt == N) {
            max = Math.max(calculate(), max);
            return;
        }

        /**
         * 순열을 이용하여 중복을 제외하고 모든 순서를 고려한 경우의 수를 계산해본다
         */
        for (int i = 0; i < N; i++) {
            if (!visit[i]) {
                visit[i] = true;
                newArr[cnt] = arr[i];
                dfs(cnt + 1);
                visit[i] = false;
            }
        }
    }

    /* 주어진 문제의 식을 이용해 계산한 메서드! */
    public static int calculate() {
        int answer = 0;
        for (int i = 0; i < N-1; i++) {
            answer += Math.abs(newArr[i] - newArr[i+1]);
        }
        return answer;
    }
}
```

