# [문제링크](https://www.acmicpc.net/problem/1717)

## 📝 문제

초기에 $n+1$개의 집합 $\{0\}, \{1\}, \{2\}, \dots , \{n\}$이 있다. 여기에 합집합 연산과, 두 원소가 같은 집합에 포함되어 있는지를 확인하는 연산을 수행하려고 한다.

집합을 표현하는 프로그램을 작성하시오.

## 입력

첫째 줄에 $n$, $m$이 주어진다. $m$은 입력으로 주어지는 연산의 개수이다. 다음 $m$개의 줄에는 각각의 연산이 주어진다. 합집합은 $0$ $a$ $b$의 형태로 입력이 주어진다. 이는 $a$가 포함되어 있는 집합과, $b$가 포함되어 있는 집합을 합친다는 의미이다. 두 원소가 같은 집합에 포함되어 있는지를 확인하는 연산은 $1$ $a$ $b$의 형태로 입력이 주어진다. 이는 $a$와 $b$가 같은 집합에 포함되어 있는지를 확인하는 연산이다.

## 출력

1로 시작하는 입력에 대해서 $a$와 $b$가 같은 집합에 포함되어 있으면 "`YES`" 또는 "`yes`"를, 그렇지 않다면 "`NO`" 또는 "`no`"를 한 줄에 하나씩 출력한다.

## 제한

-    $1 ≤ n ≤ 1\,000\,000$ 
-   $1 ≤ m ≤ 100\,000$ 
-    $0 ≤ a, b ≤ n$ 
-    $a$, $b$는 정수
-    $a$와 $b$는 같을 수도 있다.

## 예제 입력 1 

7 8
0 1 3
1 1 7
0 7 6
1 7 1
0 3 7
0 4 2
0 1 1
1 1 1

## 예제 출력 1 

NO
NO
YES

---

### 🔍 정답 (유니온 파인드)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int[] parent;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());

        // 부모 노드를 자기 자신으로 초기화
        parent = new int[n+1];
        for (int i = 0; i < n+1; i++) {
            parent[i] = i;
        }

        for (int i = 0; i < m; i++) {
            st = new StringTokenizer(br.readLine());
            int input = Integer.parseInt(st.nextToken());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());

            // 합집합 연산(union)
            if (input == 0) {
                union(a, b);
            } else {
                System.out.println(isSameParent(a, b) ? "YES" : "NO");
            }
        }
    }

    /**
     * x의 부모 노드를 찾을 때까지 재귀
     */
    public static int find(int x) {
        if (x == parent[x]) {
            return x;
        }
        return parent[x] = find(parent[x]);
    }

    /**
     * 합집합 연산
     * find 연산을 통해 부모 노드를 찾고
     * 두 노드 중 더 작은 값에 포함되도록 한다.
     */
    public static void union(int a, int b) {
        a = find(a);
        b = find(b);

        if (a != b) {
            if (a < b) {
                parent[b] = a;
            } else {
                parent[a] = b;
            }
        }
    }

    public static boolean isSameParent(int a, int b) {
        a = find(a);
        b = find(b);

        if (a == b) {
            return true;
        }

        return false;
    }
}
```
- 2차원 boolean 배열로 arr = \[현재노드\]\[부모노드\] 로 같은 그래프임을 판별해보려고 하였으나 메모리 초과가 떴다 ㅠㅠ
- 메모리를 줄일 껀덕지가 없는 것 같아서 뭔가 최적화된 알고리즘이 있을 거라고 생각을 했고 찾아보니 역시나, 유니온 파인드라는 알고리즘을 접하게 되었고 해당 내용을 공부한 뒤 풀어보았다!
- [유니온 파인드 알고리즘](/Algorithm/유니온_파인드(Union-Find).md)

### ❌ 오답 (메모리 초과, 2차원 배열)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());

        boolean[][] check = new boolean[n+1][n+1];
        for (int i = 0; i < n+1; i++) {
            for (int j = 0; j < n+1; j++) {
                if (i == j)
                    check[i][j] = true;
            }
        }

        for (int i = 0; i < m; i++) {
            st = new StringTokenizer(br.readLine());
            int input = Integer.parseInt(st.nextToken());
            int a = Integer.parseInt(st.nextToken());
            int b = Integer.parseInt(st.nextToken());

            if (input == 0) {
                check[a][b] = check[b][a] = true;
            } else {
                if (check[a][b]) System.out.println("YES");
                else System.out.println("NO");
            }
        }
    }
}
```