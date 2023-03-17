# [문제링크](https://www.acmicpc.net/problem/13023)

## 📝 문제

BOJ 알고리즘 캠프에는 총 N명이 참가하고 있다. 사람들은 0번부터 N-1번으로 번호가 매겨져 있고, 일부 사람들은 친구이다.

오늘은 다음과 같은 친구 관계를 가진 사람 A, B, C, D, E가 존재하는지 구해보려고 한다.

-   A는 B와 친구다.
-   B는 C와 친구다.
-   C는 D와 친구다.
-   D는 E와 친구다.

위와 같은 친구 관계가 존재하는지 안하는지 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 사람의 수 N (5 ≤ N ≤ 2000)과 친구 관계의 수 M (1 ≤ M ≤ 2000)이 주어진다.

둘째 줄부터 M개의 줄에는 정수 a와 b가 주어지며, a와 b가 친구라는 뜻이다. (0 ≤ a, b ≤ N-1, a ≠ b) 같은 친구 관계가 두 번 이상 주어지는 경우는 없다.

## 출력

문제의 조건에 맞는 A, B, C, D, E가 존재하면 1을 없으면 0을 출력한다.

## 예제 입력 1 

5 4
0 1
1 2
2 3
3 4

## 예제 출력 1 

1

## 예제 입력 2 

5 5
0 1
1 2
2 3
3 0
1 4

## 예제 출력 2 

1

## 예제 입력 3

6 5
0 1
0 2
0 3
0 4
0 5

## 예제 출력 3 

0

## 예제 입력 4 

8 8
1 7
3 7
4 7
3 4
4 6
3 5
0 4
2 7

## 예제 출력 4 

1

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.StringTokenizer;

public class Main {
    static int N, M;
    static ArrayList<Integer>[] list;
    static boolean[] visit;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());   // 사람의 수
        M = Integer.parseInt(st.nextToken());   // 친구관계의 수

        list = new ArrayList[N];

        for (int i = 0; i < N; i++) {
            list[i] = new ArrayList<>();
        }

        // 서로 친구관계인 경우 각각 리스트에 추가해주기
        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());

            list[a].add(b);
            list[b].add(a);
        }

        for (int i = 0; i < N; i++) {
            visit = new boolean[N];
            dfs(i, 0);
        }

        System.out.println(0);
    }

    public static void dfs(int num, int depth) {
        // 친구 관계인 경우를 dfs로 깊이 탐색하다가 깊이가 4가 되면 종료!
        if (depth == 4) {
            System.out.println(1);
            System.exit(0);
        }

        visit[num] = true;

        for (int i : list[num]) {
            if (!visit[i]) {
                dfs(i, depth+1);
                visit[i] = false;
            }
        }
    }
}
```
- 문제가 애매한데 결국 묻는 것은 한 번에 이어지는 친구 사이가 4쌍 이상이면 1, 그 미만이면 0을 출력하는 것이다.
- 예를 들어서 1 -> 2 - > 3 - > 4 -> 5 로 이어지면 12, 23, 34, 45 네 쌍이 되므로 1을 출력하는 것이고 01, 02, 03, 04, 05 라면 한 번에 이어지는 쌍이 1밖에 되지 않으므로 0을 출력하는 것이다.
- 깊이 우선 탐색으로 depth가 4 이상이 되는지를 체크하면 되는 문제!