# [문제링크](https://www.acmicpc.net/problem/1068)

## 📝 문제

트리에서 리프 노드란, 자식의 개수가 0인 노드를 말한다.

트리가 주어졌을 때, 노드 하나를 지울 것이다. 그 때, 남은 트리에서 리프 노드의 개수를 구하는 프로그램을 작성하시오. 노드를 지우면 그 노드와 노드의 모든 자손이 트리에서 제거된다.

예를 들어, 다음과 같은 트리가 있다고 하자.

![](https://upload.acmicpc.net/560de878-d961-475e-ada4-e1f0774e5a84/-/preview/)

현재 리프 노드의 개수는 3개이다. (초록색 색칠된 노드) 이때, 1번을 지우면, 다음과 같이 변한다. 검정색으로 색칠된 노드가 트리에서 제거된 노드이다.

![](https://upload.acmicpc.net/d46ddf4e-1b82-44cc-8c90-12f76e5bf88f/-/preview/)

이제 리프 노드의 개수는 1개이다.

## 입력

첫째 줄에 트리의 노드의 개수 N이 주어진다. N은 50보다 작거나 같은 자연수이다. 둘째 줄에는 0번 노드부터 N-1번 노드까지, 각 노드의 부모가 주어진다. 만약 부모가 없다면 (루트) -1이 주어진다. 셋째 줄에는 지울 노드의 번호가 주어진다.

## 출력

첫째 줄에 입력으로 주어진 트리에서 입력으로 주어진 노드를 지웠을 때, 리프 노드의 개수를 출력한다.

## 예제 입력 1 

5
-1 0 0 1 1
2

## 예제 출력 1 
2

## 예제 입력 2 

5
-1 0 0 1 1
1

## 예제 출력 2 

1

## 예제 입력 3 

5
-1 0 0 1 1
0

## 예제 출력 3 

0

## 예제 입력 4 

9
-1 0 0 2 2 4 4 6 6
4

## 예제 출력 4 

2

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N, del;
    static ArrayList<Integer>[] arr;
    static boolean[] isDelete;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());

        /**
         * 배열 N개 만들기
         * 각 배열에는 자식 노드를 넣어줄 것이다.
         * 만약에 입력값이 -1 0 0 1 1 이라면, arr[1] = [3, 4]가 될 것이고
         * 지울 노드 번호가 1번이라면 자식 노드들도 방문하기 위함이다!
         */
        arr = new ArrayList[N];
        for (int i = 0; i < N; i++) {
            arr[i] = new ArrayList<>();
        }

        /**
         * 자식 노드 넣어주기!
         */
        StringTokenizer st = new StringTokenizer(br.readLine());
        for (int i = 0; i < N; i++) {
            int parent = Integer.parseInt(st.nextToken());
            if (parent != -1) arr[parent].add(i);
        }

        del = Integer.parseInt(br.readLine());

        isDelete = new boolean[N];      // 삭제된 노드인지 확인하기 위함!

        bfs(del);

        /**
         * 배열들을 확인해서 삭제된 노드가 포함되어있다면 삭제해준다.
         * 지울 노드를 입력받은 후 지울 노드 + 지울 노드에 포함된 자식 노드들을 모두 삭제하는데(bfs)
         * 그 조건에 포함되지 않는 배열 중에서 지워지는 노드를 포함하고 있을 수 있기 때문이다.
         */
        for (int i = 0; i < N; i++) {
            if (!arr[i].isEmpty()) {
                for (int j = 0; j < arr[i].size(); j++) {
                    if (isDelete[arr[i].get(j)]) {
                        arr[i].remove(arr[i].get(j));
                    }
                }
            }
        }

        /**
         * 지워지지 않은 노드 중 비어있는(자식 노드가 없는 리프 노드) 개수만큼 카운팅
         */
        int cnt = 0;
        for (int i = 0; i < N; i++) {
            if (arr[i].isEmpty() && !isDelete[i]) cnt++;
        }

        System.out.println(cnt);
    }

    public static void bfs(int num) {
        Queue<Integer> q = new LinkedList<>();
        isDelete[num] = true;

        /**
         * 지울 노드로 입력받은 배열의 자식 노드들도 삭제되어야 하므로 큐에 넣어준다.
         */
        while (arr[num].size() > 0) {
            q.offer(arr[num].get(0));
            isDelete[arr[num].get(0)] = true;
            arr[num].remove(arr[num].get(0));
        }

        /**
         * 자식 노드들과 자식 노드들에 포함된 노드들까지 삭제하기
         */
        while (!q.isEmpty()) {
            int temp = q.poll();

            while (arr[temp].size() > 0) {
                isDelete[arr[temp].get(0)] = true;
                q.offer(arr[temp].get(0));
                arr[temp].remove(arr[temp].get(0));
            }
        }
    }
}
```


### 🔎 다른 풀이

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N, del;
    static ArrayList<Integer>[] arr;
    static int result = 0;
    static int start = 0;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());

        /**
         * 배열 N개 만들기
         * 각 배열에는 자식 노드를 넣어줄 것이다.
         * 만약에 입력값이 -1 0 0 1 1 이라면, arr[1] = [3, 4]가 될 것이고
         * 지울 노드 번호가 1번이라면 자식 노드들도 방문하기 위함이다!
         */
        arr = new ArrayList[N];
        for (int i = 0; i < N; i++) {
            arr[i] = new ArrayList<>();
        }

        /**
         * 자식 노드 넣어주고 루트 노드 확인!
         */
        StringTokenizer st = new StringTokenizer(br.readLine());
        for (int i = 0; i < N; i++) {
            int parent = Integer.parseInt(st.nextToken());
            if (parent != -1) arr[parent].add(i);
            else start = i;
        }

        del = Integer.parseInt(br.readLine());

        dfs(del);
        System.out.println(result);

    }

    public static void dfs(int del) {
        Queue<Integer> q = new LinkedList<>();

        // 만약 삭제할 노드가 루트 노드라면 바로 리턴해서 0을 출력한다.
        if (start == del) return;

        // 루트 노드부터 탐색!
        q.offer(start);

        while (!q.isEmpty()) {
            int node = q.poll();
            int count = 0;

            /**
             * 해당 노드의 자식노드들을 확인해서 삭제 노드라면 방문하지 않고
             * 삭제 노드가 아니라면 큐에 넣어준다.
             * 이 때, 큐에 넣는 자식 노드가 있는 노드라면 카운팅을 해서 리프 노드로 세지 않도록 하고
             * 큐에 넣을 게 없는 노드라면 리프 노드이므로 카운팅을 한다.
             */
            for (int temp : arr[node]) {
                if (temp != del) {
                    q.offer(temp);
                    count++;
                }
            }
            
            if (count == 0) result++;
        }
    }
}
```

- 내 풀이는 삭제할 노드부터 시작해서 삭제할 노드에 포함된 자식 노드들까지 모두 삭제를 한 후에 다시 전체를 돌면서 다른 노드에 포함된 삭제된 노드들을 삭제하는 방법이다.
- 반복문 개수도 많고 직관적이지 못하다고 생각해서 다른 풀이를 찾아봤는데 같은 bfs를 사용했는데 훨씬 간결하고 직관적이다...
- 내 풀이는 문제를 하나하나 순차적으로 해결하려고 한 느낌이고 다른 풀이는 문제를 단순화해서 지름길로 간 느낌이다.
- 제출해보면 메모리나 시간은 거의 차이가 없고 오히려 내 풀이가 더 적게 들었는데 왜 그런 건지 궁금하다 ㅠㅠ
- 어쨌든.... 문제를 보이는 그대로 풀려고 하기보다 단순화할 수 있는 방법을 고민해보는 게 좋을 것 같다는 생각이 들었다.