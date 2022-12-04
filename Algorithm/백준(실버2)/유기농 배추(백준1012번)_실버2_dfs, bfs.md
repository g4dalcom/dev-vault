# [문제링크]()

## 📝 문제

차세대 영농인 한나는 강원도 고랭지에서 유기농 배추를 재배하기로 하였다. 농약을 쓰지 않고 배추를 재배하려면 배추를 해충으로부터 보호하는 것이 중요하기 때문에, 한나는 해충 방지에 효과적인 배추흰지렁이를 구입하기로 결심한다. 이 지렁이는 배추근처에 서식하며 해충을 잡아 먹음으로써 배추를 보호한다. 특히, 어떤 배추에 배추흰지렁이가 한 마리라도 살고 있으면 이 지렁이는 인접한 다른 배추로 이동할 수 있어, 그 배추들 역시 해충으로부터 보호받을 수 있다. 한 배추의 상하좌우 네 방향에 다른 배추가 위치한 경우에 서로 인접해있는 것이다.

한나가 배추를 재배하는 땅은 고르지 못해서 배추를 군데군데 심어 놓았다. 배추들이 모여있는 곳에는 배추흰지렁이가 한 마리만 있으면 되므로 서로 인접해있는 배추들이 몇 군데에 퍼져있는지 조사하면 총 몇 마리의 지렁이가 필요한지 알 수 있다. 예를 들어 배추밭이 아래와 같이 구성되어 있으면 최소 5마리의 배추흰지렁이가 필요하다. 0은 배추가 심어져 있지 않은 땅이고, 1은 배추가 심어져 있는 땅을 나타낸다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvGUbD%2FbtrSJnbsP0X%2FRYtN4JwDa7i7w6I0REkZjk%2Fimg.png)

## 입력

입력의 첫 줄에는 테스트 케이스의 개수 T가 주어진다. 그 다음 줄부터 각각의 테스트 케이스에 대해 첫째 줄에는 배추를 심은 배추밭의 가로길이 M(1 ≤ M ≤ 50)과 세로길이 N(1 ≤ N ≤ 50), 그리고 배추가 심어져 있는 위치의 개수 K(1 ≤ K ≤ 2500)이 주어진다. 그 다음 K줄에는 배추의 위치 X(0 ≤ X ≤ M-1), Y(0 ≤ Y ≤ N-1)가 주어진다. 두 배추의 위치가 같은 경우는 없다.

## 출력

각 테스트 케이스에 대해 필요한 최소의 배추흰지렁이 마리 수를 출력한다.

## 예제 입력 1 

2
10 8 17
0 0
1 0
1 1
4 2
4 3
4 5
2 4
3 4
7 4
8 4
9 4
7 5
8 5
9 5
7 6
8 6
9 6
10 10 1
5 5

## 예제 출력 1 

5
1

## 예제 입력 2 

1
5 3 6
0 2
1 2
2 2
3 2
4 2
4 0

## 예제 출력 2 

2


---

### 🔍 정답

#### DFS풀이

```java
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
import java.util.LinkedList;  
import java.util.Queue;  
import java.util.StringTokenizer;  
  
public class Main {  
    static int T, row, col, ea;  
    static int[][] arr;  
    static boolean[][] visit;  
    static int[] dx = {0, -1, 0, 1};  
    static int[] dy = {1, 0, -1, 0};  
  
    public static void main(String[] args) throws IOException {  
  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        T = Integer.parseInt(br.readLine()); // 테스트 케이스  
  
        for (int t = 0; t < T; t++) {  
            StringTokenizer st = new StringTokenizer(br.readLine());  
            row = Integer.parseInt(st.nextToken()); // 배추밭의 가로 길이  
            col = Integer.parseInt(st.nextToken()); // 배추밭의 세로 길이  
            ea = Integer.parseInt(st.nextToken()); // 배추가 심어져있는 위치의 개수  
  
            /**  
             * arr = 배추가 있는 가로, 세로를 입력받기 위한 배열  
             *       배추가 있는 곳은 1, 없는 곳은 0이 된다.  
             * visit = 해당 배열을 방문했었는지 여부를 체크하기 위한 배열  
             */  
            arr = new int[row+1][col+1];  
            visit = new boolean[row+1][col+1];  
            int count = 0;  
  
            /**  
             * 배추가 심어져있다고 입력받은 좌표를 1로 바꾸어준다.  
             */            
             for (int i = 0; i < ea; i++) {  
                st = new StringTokenizer(br.readLine());  
                int a = Integer.parseInt(st.nextToken());  
                int b = Integer.parseInt(st.nextToken());  
                arr[a][b] = 1;  
            }  
  
            /**  
             * [0, 0] [0, 1] [0, 2] 순서로 가면서 배추가 심어져있는 곳을 발견하면 if문을 타서 dfs를 부른다!  
             */            
             for (int x = 0; x <= row; x++) {  
                for (int y = 0; y <= col; y++) {  
                    if (arr[x][y] == 1 && !visit[x][y]) {  
                        dfs(x, y);   
                        count++;  
                    }  
                }  
            }  
            System.out.println(count);  
        }  
    }  
  
    public static void dfs(int x, int y) {  
        visit[x][y] = true;  
  
        /**  
         * 해당 좌표에서 상하좌우를 방문  
         */  
        for (int i = 0; i < 4; i++) {  
            int cx = x + dx[i];  
            int cy = y + dy[i];  
  
            /**  
             * [0, 0] 좌표의 경우는 왼쪽과 위쪽은 방문할 수 없으므로 if문에서 걸러지고  
             * 배추밭의 크기를 벗어나는 경우도 if문에서 걸러진다.  
             * cx < row, cy < col 인 이유는 row-1, col-1 좌표에서 상하좌우를 방문하면서 거쳐가기 때문  
             * 주어진 좌표에서 배추가 있는 곳이며 방문하지 않은 곳이라면 재귀해서 이어진 곳은 계속 탐색한다!  
             */            
             if (cx >= 0 && cy >= 0 && cx < row && cy < col) {  
                if (arr[cx][cy] == 1 && !visit[cx][cy]) {  
                    dfs(cx, cy);  
                }  
            }  
        }  
    }  
}
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb7j91D%2FbtrSM3wcczB%2FSs5xzWtRSZtBintjP5KjQ1%2Fimg.jpg)
- 처음에 [0, 0] 좌표가 1이므로 dfs(0, 0)으로 시작합니다.
- 기준 좌표에서 상하좌우 중 한 군데를 먼저 쭉 이어진 곳이 없을 때까지 탐색하고, 기준 좌표로 돌아왔을 때 상하좌우 중 탐색하지 않은 곳을 다시 쭉 탐색하는 방식입니다!
- 그림을 예로 들면, 
- visit[0, 0] = true 로 바꾸어주고 그곳을 기준으로 상하좌우를 탐색합니다.
	- 오른쪽으로 먼저 간다고 가정하고,
	- 기준 좌표 (0, 0), arr[1]\[0] == 1 이고 !visit[1]\[0] 이므로 dfs(1, 0) 을 부릅니다.
		- 기준 좌표 (1, 0), visit[1, 0] == true로 바꾸고 상하좌우를 탐색합니다.
		- arr[1]\[1] == 1이고 !visit[1]\[1] 이므로 bfs(1, 1) 을 부릅니다.
			- 기준 좌표 (1, 1), 상하좌우 중 1이거나 방문하지 않은 곳이 없으므로 재귀 전(arr[1]\[0])으로 돌아갑니다.
		- 다시 (1, 0) 좌표로 돌아와서 상하좌우 중 안 본 곳을 탐색하는데 이제 해당하는 곳이 없으므로 재귀 전으로 돌아갑니다(arr[0]\[0])
	- 기준 좌표 (0, 0) 에서도 더이상 해당하는 곳이 없으므로 
- 메인 함수로 돌아가서 count를 세어주고 또 다른 배추가 심어진 곳을 찾으며 반복이 됩니다!


#### BFS풀이

```java
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
import java.util.LinkedList;  
import java.util.Queue;  
import java.util.StringTokenizer;  
  
public class Main {  
    static int T, row, col, ea;  
    static int[][] arr;  
    static boolean[][] visit;  
    static int[] dx = {0, -1, 0, 1};  
    static int[] dy = {1, 0, -1, 0};  
  
    public static void main(String[] args) throws IOException {  
  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        T = Integer.parseInt(br.readLine()); // 테스트 케이스  
  
        for (int t = 0; t < T; t++) {  
            StringTokenizer st = new StringTokenizer(br.readLine());  
            row = Integer.parseInt(st.nextToken()); // 배추밭의 가로 길이  
            col = Integer.parseInt(st.nextToken()); // 배추밭의 세로 길이  
            ea = Integer.parseInt(st.nextToken()); // 배추가 심어져있는 위치의 개수  
  
            /**  
             * arr = 배추가 있는 가로, 세로를 입력받기 위한 배열  
             *       배추가 있는 곳은 1, 없는 곳은 0이 된다.  
             * visit = 해당 배열을 방문했었는지 여부를 체크하기 위한 배열  
             */  
            arr = new int[row+1][col+1];  
            visit = new boolean[row+1][col+1];  
            int count = 0;  
  
            /**  
             * 배추가 심어져있다고 입력받은 좌표를 1로 바꾸어준다.  
             */            
             for (int i = 0; i < ea; i++) {  
                st = new StringTokenizer(br.readLine());  
                int a = Integer.parseInt(st.nextToken());  
                int b = Integer.parseInt(st.nextToken());  
                arr[a][b] = 1;  
            }  
  
            /**  
             * [0, 0] [0, 1] [0, 2] 순서로 가면서 배추가 심어져있는 곳을 발견하면 if문을 타서 bfs를 부른다!  
             */            
             for (int x = 0; x <= row; x++) {  
                for (int y = 0; y <= col; y++) {  
                    if (arr[x][y] == 1 && !visit[x][y]) {  
                        bfs(x, y);  
                        count++;  
                    }  
                }  
            }  
            System.out.println(count);  
        }  
    }  
  
    public static void bfs(int x, int y) {  
        Queue<int[]> q = new LinkedList<>();  
        q.offer(new int[] {x, y});  
  
        visit[x][y] = true;  
  
        while (!q.isEmpty()) {  
  
            x = q.peek()[0];  
            y = q.peek()[1];  
            q.poll();  
  
            for (int i = 0; i < 4; i++) {  
                int cx = x + dx[i];  
                int cy = y + dy[i];  
  
                if (cx >= 0 && cy >= 0 && cx < row && cy < col) {  
                    if (arr[cx][cy] == 1 && !visit[cx][cy]) {  
                        q.offer(new int[] {cx, cy});  
                        visit[cx][cy] = true;  
                    }  
                }  
            }  
        }  
    }  
}
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FUaFSp%2FbtrSOxDMSCJ%2FyKptq1nt4OdQi1kuSjFAO0%2Fimg.jpg)
- bfs 역시 처음에 bfs(0, 0)을 부르면서 시작합니다.
- int[] 형 Queue에 {0, 0}을 넣어주고 visit[0]\[0] = true를 해줍니당.
- 그리고 큐가 빌 때까지 while문을 반복할 것입니다.
	- dfs와 마찬가지로 기준 좌표에서 상하좌우를 방문하는데 배추가 심어졌으며 방문하지 않았던 곳은 q에 좌표를 넣어줍니다.
	- q = [{0, 0}] 에서 q = [{1, 0}]이 될 것이고 큐가 비지 않았으니 x, y 값을 갱신하고 그 값을 기준으로 다시 상하좌우를 탐색합니다.
	- 그림에서는 큐에 하나씩 넣었다 빠졌다 하겠지만, 만약 오른쪽과 아래쪽 두군데가 해당한다면 q = [{1, 0}, {1, 1}] 이런 상태가 될 수 있고
	- 저 상황에서 다시 while문을 돈다면 {1, 0} 부터 x, y값으로 갱신되어서 상하좌우 탐색을 하고 배추가 위치한 좌표가 있다면 좌표는 {1, 1} 뒤로 q에 담겨질 것이고
	- {1, 0} 의 상하좌우를 다 탐색했다면 다시 {1, 1} 로 x, y값이 갱신되어서 상하좌우 탐색을 할 것입니다(반복)