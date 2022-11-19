# [문제링크](https://www.acmicpc.net/problem/2606)

## 📝 문제

신종 바이러스인 웜 바이러스는 네트워크를 통해 전파된다. 한 컴퓨터가 웜 바이러스에 걸리면 그 컴퓨터와 네트워크 상에서 연결되어 있는 모든 컴퓨터는 웜 바이러스에 걸리게 된다.

예를 들어 7대의 컴퓨터가 <그림 1>과 같이 네트워크 상에서 연결되어 있다고 하자. 1번 컴퓨터가 웜 바이러스에 걸리면 웜 바이러스는 2번과 5번 컴퓨터를 거쳐 3번과 6번 컴퓨터까지 전파되어 2, 3, 5, 6 네 대의 컴퓨터는 웜 바이러스에 걸리게 된다. 하지만 4번과 7번 컴퓨터는 1번 컴퓨터와 네트워크상에서 연결되어 있지 않기 때문에 영향을 받지 않는다.

![](https://www.acmicpc.net/upload/images/zmMEZZ8ioN6rhCdHmcIT4a7.png)

어느 날 1번 컴퓨터가 웜 바이러스에 걸렸다. 컴퓨터의 수와 네트워크 상에서 서로 연결되어 있는 정보가 주어질 때, 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 출력하는 프로그램을 작성하시오.

## 입력

첫째 줄에는 컴퓨터의 수가 주어진다. 컴퓨터의 수는 100 이하이고 각 컴퓨터에는 1번 부터 차례대로 번호가 매겨진다. 둘째 줄에는 네트워크 상에서 직접 연결되어 있는 컴퓨터 쌍의 수가 주어진다. 이어서 그 수만큼 한 줄에 한 쌍씩 네트워크 상에서 직접 연결되어 있는 컴퓨터의 번호 쌍이 주어진다.

## 출력

1번 컴퓨터가 웜 바이러스에 걸렸을 때, 1번 컴퓨터를 통해 웜 바이러스에 걸리게 되는 컴퓨터의 수를 첫째 줄에 출력한다.

## 예제 입력 1

7
6
1 2
2 3
1 5
5 2
5 6
4 7

## 예제 출력 1 

4


---

### 🔍 정답(DFS)

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
    static boolean[] visit;
    static int[][] arr;
    static int cnt = 0;
    static int node, line;

    public static void main(String[] args) throws IOException {

        /**
         * 인접 노드 입력받기
         * 입력값으로 받은 인접노드들은 1이라는 값을 갖게 된다!
         * col 3, row 4이고 인접노드를 (1, 2) (2, 3) 이라고 가정하면 아래와 같이 만들어질 것이다.
         *      1   2   3   4
         * 1    0   1   0   0
         * 2    1   0   1   0
         * 3    0   1   0   0
         * arr[col][row] = arr[row][col] = 1; 은 [1 ,2]나 [2, 1]이나 같기 때문이다.
         */
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        node = Integer.parseInt(br.readLine());
        line = Integer.parseInt(br.readLine());

        arr = new int[node+1][node+1];
        visit = new boolean[node+1];

        for (int i = 0; i < line; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int col = Integer.parseInt(st.nextToken());
            int row = Integer.parseInt(st.nextToken());

            arr[col][row] = arr[row][col] = 1;
        }
        /**
         * 1부터 dfs 메서드 수행하기!
         */
        dfs(1);

        /**
         * 1번 컴퓨터도 cnt를 세어주었으므로 cnt에서 1을 빼준다!
         */
        System.out.println(cnt - 1);
    }

    public static void dfs(int num) {
        /**
         * 처음에 1을 넣고 수행하며
         * 그 다음부터는 아래 "인접행렬 확인 메서드"를 통해 들어온 값(인접해서 바이러스에 걸린 뇨속)의 방문을 true로 바꾸고 cnt를 세어준다.
         */
        visit[num] = true;
        cnt++;

        /**
         * 인접행렬 확인하기
         * 인접행렬이면서 방문하지 않았다면 그 수를 가지고 다시 dfs 메서드를 수행한다.
         */
        for (int i = 0; i <= node; i++) {
            if (arr[num][i] == 1 && !visit[i]) {
                dfs(i);
            }
        }
    }
}
```

### 🔍 정답(BFS)
```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {

        /**
         * 기본 접근법
         * 인접노드들의 값을 1로, 그렇지 않은 노드들은 0으로 주고
         * 값이 1이면서 방문한 적 없는(방문배열로 확인) 값들을 Queue배열에 넣고 하나씩 빼면서 인접노드를 확인!
         * Queue에 들어간 수들은 바이러스에 감염된 뇨속들이고 그 뇨속들과 인접한 노드들을 계속 찾기 위함이다.
         *

        /**
         * 인접 노드 입력받기
         * 입력값으로 받은 인접노드들은 1이라는 값을 갖게 된다!
         * col 3, row 4이고 인접노드를 (1, 2) (2, 3) 이라고 가정하면 아래와 같이 만들어질 것이다.
         *      1   2   3   4
         * 1    0   1   0   0
         * 2    1   0   1   0
         * 3    0   1   0   0
         * arr[col][row] = arr[row][col] = 1; 은 [1 ,2]나 [2, 1]이나 같기 때문이다.
         */
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int node = Integer.parseInt(br.readLine());
        int line = Integer.parseInt(br.readLine());

        int[][] arr = new int[node+1][node+1];
        for (int i = 0; i < line; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int col = Integer.parseInt(st.nextToken());
            int row = Integer.parseInt(st.nextToken());

            arr[col][row] = arr[row][col] = 1;
        }

        /**
         * Queue 배열 선언 후 1을 넣어준다 Q = [1]
         * visit 방문 배열 선언 후 true로 바꾸어준다. visit = [true, false, false, false ...]
         */
        Queue<Integer> Q = new LinkedList<>();
        boolean[] visit = new boolean[node+1];

        Q.add(1);
        visit[1] = true;

        /**
         * 바이러스에 걸린 컴퓨터의 수를 세어주기 위해 cnt 변수를 만들고
         * num이라는 변수에 Q에 들어가있는 값을 꺼낸다(처음에 1이 꺼내진다)
         * arr[1][i] 을 돌면서 값이 1이면서 방문한 적 없는 숫자가 나오면
         * 방문배열을 true로 바꾸어주고 Q에 해당 값을 넣고 cnt를 세어준다.
         * 만약 1과 인접한 노드가 arr[1][2], arr[1][3] 이라면 Q에는 2, 3이 들어가고
         * 2부터 꺼내어져서 3까지 while문을 수행하게 된다.
         */
        int cnt = 0;
        while (!Q.isEmpty()) {
            int num = Q.poll();
            for (int i = 1; i <= node; i++) {
                if (arr[num][i] == 1 && !visit[i]) {
                    visit[i] = true;
                    Q.add(i);
                    cnt++;
                }
            }
        }
        System.out.println(cnt);
    }
}
```