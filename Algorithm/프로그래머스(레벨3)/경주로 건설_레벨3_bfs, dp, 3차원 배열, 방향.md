# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/67259)

## 📝 문제

건설회사의 설계사인 `죠르디`는 고객사로부터 자동차 경주로 건설에 필요한 견적을 의뢰받았습니다.  
제공된 경주로 설계 도면에 따르면 경주로 부지는 `N x N` 크기의 정사각형 격자 형태이며 각 격자는 `1 x 1` 크기입니다.  
설계 도면에는 각 격자의 칸은 `0` 또는 `1` 로 채워져 있으며, `0`은 칸이 비어 있음을 `1`은 해당 칸이 벽으로 채워져 있음을 나타냅니다.  
경주로의 출발점은 (0, 0) 칸(좌측 상단)이며, 도착점은 (N-1, N-1) 칸(우측 하단)입니다. 죠르디는 출발점인 (0, 0) 칸에서 출발한 자동차가 도착점인 (N-1, N-1) 칸까지 무사히 도달할 수 있게 중간에 끊기지 않도록 경주로를 건설해야 합니다.  
경주로는 상, 하, 좌, 우로 인접한 두 빈 칸을 연결하여 건설할 수 있으며, 벽이 있는 칸에는 경주로를 건설할 수 없습니다.  
이때, 인접한 두 빈 칸을 상하 또는 좌우로 연결한 경주로를 `직선 도로` 라고 합니다.  
또한 두 `직선 도로`가 서로 직각으로 만나는 지점을 `코너` 라고 부릅니다.  
건설 비용을 계산해 보니 `직선 도로` 하나를 만들 때는 100원이 소요되며, `코너`를 하나 만들 때는 500원이 추가로 듭니다.  
죠르디는 견적서 작성을 위해 경주로를 건설하는 데 필요한 최소 비용을 계산해야 합니다.

예를 들어, 아래 그림은 `직선 도로` 6개와 `코너` 4개로 구성된 임의의 경주로 예시이며, 건설 비용은 6 x 100 + 4 x 500 = 2600원 입니다.

![kakao_road2.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/0e0911e8-f88e-44fe-8bdc-6856a56df8e0/kakao_road2.png)

또 다른 예로, 아래 그림은 `직선 도로` 4개와 `코너` 1개로 구성된 경주로이며, 건설 비용은 4 x 100 + 1 x 500 = 900원 입니다.

![kakao_road3.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/3f5d9c5e-d7d9-4248-b111-140a0847e741/kakao_road3.png)

---

도면의 상태(0은 비어 있음, 1은 벽)을 나타내는 2차원 배열 board가 매개변수로 주어질 때, 경주로를 건설하는데 필요한 최소 비용을 return 하도록 solution 함수를 완성해주세요.

##### **[제한사항]**

- board는 2차원 정사각 배열로 배열의 크기는 3 이상 25 이하입니다.
- board 배열의 각 원소의 값은 0 또는 1 입니다.
    - 도면의 가장 왼쪽 상단 좌표는 (0, 0)이며, 가장 우측 하단 좌표는 (N-1, N-1) 입니다.
    - 원소의 값 0은 칸이 비어 있어 도로 연결이 가능함을 1은 칸이 벽으로 채워져 있어 도로 연결이 불가능함을 나타냅니다.
- board는 항상 출발점에서 도착점까지 경주로를 건설할 수 있는 형태로 주어집니다.
- 출발점과 도착점 칸의 원소의 값은 항상 0으로 주어집니다.

---

##### **입출력 예**

|board|result|
|---|---|
|[[0,0,0],[0,0,0],[0,0,0]]|900|
|[[0,0,0,0,0,0,0,1],[0,0,0,0,0,0,0,0],[0,0,0,0,0,1,0,0],[0,0,0,0,1,0,0,0],[0,0,0,1,0,0,0,1],[0,0,1,0,0,0,1,0],[0,1,0,0,0,1,0,0],[1,0,0,0,0,0,0,0]]|3800|
|[[0,0,1,0],[0,0,0,0],[0,1,0,1],[1,0,0,0]]|2100|
|[[0,0,0,0,0,0],[0,1,1,1,1,0],[0,0,1,0,0,0],[1,0,0,1,0,1],[0,1,0,0,0,1],[0,0,0,0,0,0]]|3200|

##### **입출력 예에 대한 설명**

**입출력 예 #1**

본문의 예시와 같습니다.

**입출력 예 #2**

![kakao_road4.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/ccc72e9c-2e22-4a09-a94b-ff057b081a70/kakao_road4.png)

위와 같이 경주로를 건설하면 `직선 도로` 18개, `코너` 4개로 총 3800원이 듭니다.

**입출력 예 #3**

![kakao_road5.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/422e86e0-a7d7-4a09-9b42-2b6218a9b5f0/kakao_road5.png)

위와 같이 경주로를 건설하면 `직선 도로` 6개, `코너` 3개로 총 2100원이 듭니다.

**입출력 예 #4**

![kakao_road6.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/4fe42f47-2592-4cb8-91fb-31d6a6da8639/kakao_road6.png)

붉은색 경로와 같이 경주로를 건설하면 `직선 도로` 12개, `코너` 4개로 총 3200원이 듭니다.  
만약, 파란색 경로와 같이 경주로를 건설한다면 `직선 도로` 10개, `코너` 5개로 총 3500원이 들며, 더 많은 비용이 듭니다.

---

### 💡 풀이

- 일반적인 최단거리를 구하는 문제의 응용 버전입니다. 그런데 생각보다 구현하는 게 까다로웠습니다 ㅠㅠ
- 우선은 가중치만 있는 최단거리를 구한다고 하면 input 배열과 동일한 크기의 배열로 최소값을 갱신해가는 방식으로 구현할 수 있습니다. 이 때 필요한 것은 x, y라는 좌표값과 가중치겠죠!
- 그리고 가중치가 다른 최단거리는 큐를 이용한 BFS로 구할 수 있습니다.
- 이 문제는 여기서 **방향** 이라는 속성이 하나 더 등장합니다. 같은 (1, 1) 좌표여도 이 좌표로 들어오는 방향이 위였는지 왼쪽이었는지 등에 따라 구현이 달라져야 하는 것입니다.
- 따라서 Road 라는 클래스에도 direction 이라는 속성을 추가해서 구분할 수 있도록 하였습니다.
- 기본적인 접근법은 현재 위치에서 갈 수 있는 방향으로 가되(q.add()) 방향 비교라는 작업을 한 번 더 하는 것입니다.

```java
if (dp[i][cx][cy] > dp[current.direction][current.x][current.y] + cost) {
	dp[i][cx][cy] = dp[current.direction][current.x][current.y] + cost;
	q.add(new Road(cx, cy, i, current.cost + cost));
```

- q에서 빼낸 현재 위치(current) 의 방향(current.direction) 과 다음 가려는 위치(cx, cy) 의 방향(i) 을 비교해서 cost를 100을 줄 것인지 600을 줄 것인지 비교하는 것이고 또한 기존에 입력된 값이 있다면 더 최솟값으로 갱신해주는 것이죠!
- 그리고 (0, 0) 좌표는 어떠한 방향이 따로 없기 때문에 0, 0에서 이동할 수 있는 (0, 1)과 (1, 0)을 시작점으로 큐에 넣어주었습니다.

### 🔍 처음 풀이

```java
import java.util.*;

class Solution {
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int[][] board;
    static int[][] dp;
    static int min = Integer.MAX_VALUE;
    
    public int solution(int[][] board) {
        this.board = board;
        dp = new int[board.length][board[0].length];
        
        for (int i = 0; i < board.length; i++) {
            Arrays.fill(dp[i], Integer.MAX_VALUE);
        }

        bfs();

        return min;
    }
    
    public void bfs() {
        Queue<Road> q = new LinkedList<>();
        dp[0][0] = 0;
        q.add(new Road(0, 0, -1, 0));
        
        while (!q.isEmpty()) {
            Road current = q.poll();
            
            if (current.x == board.length-1 && current.y == board[0].length-1) {
                min = Math.min(min, current.cost);
                System.out.println(min);
                continue;
            }
            
            for (int i = 0; i < 4; i++) {
                int cx = current.x + dx[i];
                int cy = current.y + dy[i];

                if (cx >= 0 && cy >= 0 && cx < board.length && cy < board[0].length) {
                    if (board[cx][cy] == 0) {
                        int cost = 600;
                        if (current.direction == -1 || current.direction == i) cost = 100;
                        
                        if (dp[cx][cy] > current.cost + cost - 500) {
                            dp[cx][cy] = current.cost + cost;
                            q.add(new Road(cx, cy, i, current.cost + cost));   
                        }
                    }
                }
            }
        }
    }
    
    public class Road {
        int x, y, direction, cost;
        
        public Road(int x, int y, int direction, int cost) {
            this.x = x;
            this.y = y;
            this.direction = direction;
            this.cost = cost;
        }
    }
}
```

- 처음 풀이는 2차원 배열을 이용하여 풀어보았습니다. 3차원 배열로 방향을 고려한 풀이와 다른 점은 아래 부분입니다.

```java
int cost = 600;
if (current.direction == -1 || current.direction == i) cost = 100;

if (dp[cx][cy] > current.cost + cost - 500) {
	dp[cx][cy] = current.cost + cost;
	q.add(new Road(cx, cy, i, current.cost + cost)); 
```

- **dp에 저장된 값**과 **현재까지의 비용 + 방향에 따른 비용**을 비교하는 게 아니라 추가로 -500을 해주고 있죠?!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FckLzBw%2FbtsCVIh2odY%2FhKOlnj35LHVFKKFJLqM5XK%2Fimg.png)

- dp 값은 이동하면서 더 적은 값으로 갱신을 해주게 되는데 문제는, 위에서 들어오는 값과 왼쪽에서 들어오는 값 중 위에서 들어오는 값이 더 작아서 갱신이 되었다고 가정했을 때 그 다음 위치로 가는 길에서 코너가 발생해서 왼쪽에서 들어오는 값이 들어왔어야 더 작은 값이 나오는 상황이 발생할 수가 있습니다.
- 그래서 dp값을 갱신할 때는 코너가 발생하지 않았을 경우를 계산하는 작업이 필요합니다.
- 이 부분은 해결하기 정말 어려웠고 다른 분들의 풀이와 해설을 보고 인사이트를 얻을 수 있었습니다. ㅠㅠ

### 🔍 정답

```java
import java.util.*;

class Solution {
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int[][] board;
    static int[][][] dp;
    
    public int solution(int[][] board) {
        this.board = board;
        dp = new int[4][board.length][board[0].length];
        
        for (int i = 0; i < 4; i++) {
            for (int j = 0; j < board.length; j++) {
                Arrays.fill(dp[i][j], Integer.MAX_VALUE);
            }
        }

        bfs();
        
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < 4; i++) {
            min = Math.min(min, dp[i][board.length-1][board[0].length-1]);
        }
        
        return min;
    }
    
    public void bfs() {
        Queue<Road> q = new LinkedList<>();
        if (board[1][0] == 0) {
            dp[1][0][0] = 0;
            q.add(new Road(0, 0, 1, 0));
        }
        
        if (board[0][1] == 0) {
            dp[3][0][0] = 0;
            q.add(new Road(0, 0, 3, 0));
        }
        
        while (!q.isEmpty()) {
            Road current = q.poll();
                        
            for (int i = 0; i < 4; i++) {
                int cx = current.x + dx[i];
                int cy = current.y + dy[i];

                if (cx >= 0 && cy >= 0 && cx < board.length && cy < board[0].length) {
                    if (board[cx][cy] == 0) {
                        int cost = 100;
                        if (current.direction != i) cost = 600;
                        
                        if (dp[i][cx][cy] > dp[current.direction][current.x][current.y] + cost) {
                            dp[i][cx][cy] = dp[current.direction][current.x][current.y] + cost;
                            q.add(new Road(cx, cy, i, current.cost + cost));
                        }
                    }
                }
            }
        }
    }
    
    public class Road {
        int x, y, direction, cost;
        
        public Road(int x, int y, int direction, int cost) {
            this.x = x;
            this.y = y;
            this.direction = direction;
            this.cost = cost;
        }
    }
}
```