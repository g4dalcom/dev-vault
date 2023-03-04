# [문제링크](https://www.acmicpc.net/problem/3190)

## 📝 문제

 'Dummy' 라는 도스게임이 있다. 이 게임에는 뱀이 나와서 기어다니는데, 사과를 먹으면 뱀 길이가 늘어난다. 뱀이 이리저리 기어다니다가 벽 또는 자기자신의 몸과 부딪히면 게임이 끝난다.

게임은 NxN 정사각 보드위에서 진행되고, 몇몇 칸에는 사과가 놓여져 있다. 보드의 상하좌우 끝에 벽이 있다. 게임이 시작할때 뱀은 맨위 맨좌측에 위치하고 뱀의 길이는 1 이다. 뱀은 처음에 오른쪽을 향한다.

뱀은 매 초마다 이동을 하는데 다음과 같은 규칙을 따른다.

-   먼저 뱀은 몸길이를 늘려 머리를 다음칸에 위치시킨다.
-   만약 이동한 칸에 사과가 있다면, 그 칸에 있던 사과가 없어지고 꼬리는 움직이지 않는다.
-   만약 이동한 칸에 사과가 없다면, 몸길이를 줄여서 꼬리가 위치한 칸을 비워준다. 즉, 몸길이는 변하지 않는다.

사과의 위치와 뱀의 이동경로가 주어질 때 이 게임이 몇 초에 끝나는지 계산하라.

## 입력

첫째 줄에 보드의 크기 N이 주어진다. (2 ≤ N ≤ 100) 다음 줄에 사과의 개수 K가 주어진다. (0 ≤ K ≤ 100)

다음 K개의 줄에는 사과의 위치가 주어지는데, 첫 번째 정수는 행, 두 번째 정수는 열 위치를 의미한다. 사과의 위치는 모두 다르며, 맨 위 맨 좌측 (1행 1열) 에는 사과가 없다.

다음 줄에는 뱀의 방향 변환 횟수 L 이 주어진다. (1 ≤ L ≤ 100)

다음 L개의 줄에는 뱀의 방향 변환 정보가 주어지는데,  정수 X와 문자 C로 이루어져 있으며. 게임 시작 시간으로부터 X초가 끝난 뒤에 왼쪽(C가 'L') 또는 오른쪽(C가 'D')로 90도 방향을 회전시킨다는 뜻이다. X는 10,000 이하의 양의 정수이며, 방향 전환 정보는 X가 증가하는 순으로 주어진다.

## 출력

첫째 줄에 게임이 몇 초에 끝나는지 출력한다.

## 예제 입력 1 

6
3
3 4
2 5
5 3
3
3 D
15 L
17 D

## 예제 출력 1

9

## 예제 입력 2 

10
4
1 2
1 3
1 4
1 5
4
8 D
10 D
11 D
13 L

## 예제 출력 2 

21

## 예제 입력 3

10
5
1 5
1 3
1 2
1 6
1 7
4
8 D
10 D
11 D
13 L

## 예제 출력 3 

13

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N, K, L;
    static int[][] board;
    static HashMap<Integer, String> moving;
    static boolean[][] snake;
    static int seconds = 0;
    static int[] dx = {0, 1, 0, -1};
    static int[] dy = {1, 0, -1, 0};

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());
        K = Integer.parseInt(br.readLine());

        StringTokenizer st;

        board = new int[N][N];

        // 사과의 위치를 1로 표시! 좌표를 (0, 0)부터 표현하기 위해서 입력값을 각각 -1 해주었다.
        for (int i = 0; i < K; i++) {
            st = new StringTokenizer(br.readLine());
            int row = Integer.parseInt(st.nextToken());
            int col = Integer.parseInt(st.nextToken());

            board[row-1][col-1] = 1;
        }

        L = Integer.parseInt(br.readLine());

        // 뱀의 방향 변환 관련 입력값을 해시맵에 넣어주었다.
        moving = new LinkedHashMap<>();
        for (int i = 0; i < L; i++) {
            st = new StringTokenizer(br.readLine());
            int sec = Integer.parseInt(st.nextToken());
            String dir = st.nextToken();

            moving.put(sec, dir);
        }

        // 뱀의 몸통은 true로 표현
        snake = new boolean[N][N];
        dummy(0, 0, 0);
        System.out.println(seconds);
    }

    public static void dummy(int x, int y, int direction) {
        /**
         * 뱀이 이동하는 좌표에 따라 큐에 넣어주고(offer, 뒤에 넣기)
         * 뱀의 꼬리는 가장 먼저 큐에 들어간 좌표이므로 꼬리를 자를 때 (poll, 앞에서 빼기)
         * 를 위해 Deque를 이용하였다!
         */
        Deque<Location> dq = new ArrayDeque<>();
        dq.offer(new Location(x, y));
        snake[x][y] = true;

        while (true) {
            seconds++;
            Location lo = dq.peekLast();        // 뒷부분(머리)의 좌표 확인

            int cx = lo.x + dx[direction];
            int cy = lo.y + dy[direction];

            // 종료조건 : 벽에 부딪히거나 뱀의 몸통에 부딪히면 게임오버!
            if (cx < 0 || cy < 0 || cx >= N || cy >= N || snake[cx][cy]) {
                break;
            }

            // 사과 존재 여부에 따른 로직
            if (board[cx][cy] == 1) {
                board[cx][cy] = 0;
            } else {
                Location tail = dq.poll();
                snake[tail.x][tail.y] = false;
            }

            dq.offer(new Location(cx, cy));
            snake[cx][cy] = true;

            // 방향을 바꾸기 위한 로직
            if (moving.containsKey(seconds)) {
                if (moving.get(seconds).equals("D")) {
                    direction = (direction + 1) % 4;
                } else {
                    direction = (4 + (direction - 1)) % 4;
                }
            }
        }
    }

    public static class Location {
        int x, y;

        public Location(final int x, final int y) {
            this.x = x;
            this.y = y;
        }
    }
}
```
- 뱀이 방향을 바꾸어야 하는 부분을 저장하기 위해서 hashmap을 사용해보았다. 해시맵은 거의 사용해보지 않아서 사용방법을 찾아보면서 시도해보았는데 쉽지가 않았다 ㅠㅠ

```java
Iterator<Map.Entry<Integer, String>> iter = moving.entrySet().iterator();  
while (iter.hasNext()) {  
    Map.Entry<Integer, String> entry = iter.next();  
    if (seconds == entry.getKey()) {  
        if (entry.getValue().equals("D")) {  
            direction = (direction + 1) % 4;  
        } else {  
            direction = (4 + (direction - 1)) % 4;  
        }  
    }  
}
```
- LinkedHashMap을 사용한 이유는, 처음에 방향을 바꾸기 위한 로직을 위와 같이 짜기 위함이었는데 while문 안에 while문이 존재하고 전체적인 순서를 정하기가 너무 복잡했다.
- 한참 고민하다가 다른 사람의 풀이를 참조했는데.... containsKey라는 메서드가 있어서 고민을 덜 수 있었다. 다른 문제풀이에서 해시맵을 일부러 이용해보면서 익숙해져야 할 것 같다. ㅠㅠ
- [참고 사이트](https://velog.io/@kimmjieun/%EB%B0%B1%EC%A4%80-3190%EB%B2%88-%EB%B1%80-Java-%EC%9E%90%EB%B0%94)