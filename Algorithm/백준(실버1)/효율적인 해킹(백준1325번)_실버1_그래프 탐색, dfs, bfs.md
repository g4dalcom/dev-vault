# [문제링크](https://www.acmicpc.net/problem/1325)

## 📝 문제

해커 김지민은 잘 알려진 어느 회사를 해킹하려고 한다. 이 회사는 N개의 컴퓨터로 이루어져 있다. 김지민은 귀찮기 때문에, 한 번의 해킹으로 여러 개의 컴퓨터를 해킹 할 수 있는 컴퓨터를 해킹하려고 한다.

이 회사의 컴퓨터는 신뢰하는 관계와, 신뢰하지 않는 관계로 이루어져 있는데, A가 B를 신뢰하는 경우에는 B를 해킹하면, A도 해킹할 수 있다는 소리다.

이 회사의 컴퓨터의 신뢰하는 관계가 주어졌을 때, 한 번에 가장 많은 컴퓨터를 해킹할 수 있는 컴퓨터의 번호를 출력하는 프로그램을 작성하시오.

## 입력

첫째 줄에, N과 M이 들어온다. N은 10,000보다 작거나 같은 자연수, M은 100,000보다 작거나 같은 자연수이다. 둘째 줄부터 M개의 줄에 신뢰하는 관계가 A B와 같은 형식으로 들어오며, "A가 B를 신뢰한다"를 의미한다. 컴퓨터는 1번부터 N번까지 번호가 하나씩 매겨져 있다.

## 출력

첫째 줄에, 김지민이 한 번에 가장 많은 컴퓨터를 해킹할 수 있는 컴퓨터의 번호를 오름차순으로 출력한다.

## 예제 입력 1 

5 4
3 1
3 2
4 3
5 3

## 예제 출력 1 

1 2

---

### 🔍 정답(BFS)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N, M;
    static ArrayList<ArrayList<Integer>> relation = new ArrayList<>();
    static int cnt;
    static boolean[] visit;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        for (int i = 0; i <= N; i++) {
            relation.add(new ArrayList<>());
        }

        // b를 해킹하면 a도 해킹할 수 있는 것으로, b에 a가 속한다고 생각해서 b 리스트에 a를 요소로 담았다.
        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());

            relation.get(b).add(a);
        }

        int[] result = new int[N+1];
        int max = Integer.MIN_VALUE;
        for (int i = 1; i <= N; i++) {
            visit = new boolean[N+1];
            cnt = 0;
            result[i] = bfs(i);
            max = Math.max(max, result[i]);

        }

        // max값이 여러개인 경우 출력을 위함!
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < result.length; i++) {
            if (max == result[i]) {
                sb.append(i).append(" ");
            }
        }

        System.out.println(sb);
    }

    public static int bfs(int num) {
        Queue<Integer> q = new LinkedList<>();
        visit[num] = true;
        q.offer(num);

        while (!q.isEmpty()) {
            int cur = q.poll();

            for (int i: relation.get(cur)) {
                if (!visit[i]) {
                    visit[i] = true;
                    q.offer(i);
                    cnt++;
                }
            }
        }
        return cnt;
    }
}
```


### 🔎 정답(DFS)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N, M;
    static ArrayList<ArrayList<Integer>> relation = new ArrayList<>();
    static boolean[] visit;
    static int[] dp;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        for (int i = 0; i <= N; i++) {
            relation.add(new ArrayList<>());
        }

        // b를 해킹하면 a도 해킹이 가능하다고 할 때, b를 부모, a를 자식 컴퓨터라고 가정하고 재귀로 부모 컴퓨터를 세어주기 위함!
        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());

            relation.get(a).add(b);
        }

        dp = new int[N+1];

        for (int i = 1; i <= N; i++) {
            visit = new boolean[N+1];
            dfs(i);
        }

        int max = Integer.MIN_VALUE;
        for (int i = 0; i < dp.length; i++) {
            if (max < dp[i]) {
                max = dp[i];
            }
        }

        // max값이 여러개인 경우 출력을 위함!
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < dp.length; i++) {
            if (max == dp[i]) {
                sb.append(i).append(" ");
            }
        }

        System.out.println(sb);
    }

    public static void dfs(int num) {
        visit[num] = true;

        for (int i : relation.get(num)) {
            if (!visit[i]) {
                dp[i]++;
                dfs(i);
            }
        }
    }
}
```