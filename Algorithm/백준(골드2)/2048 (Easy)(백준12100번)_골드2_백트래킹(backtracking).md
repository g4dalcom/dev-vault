# [문제링크](https://www.acmicpc.net/problem/12100)

## 📝 문제

2048 게임은 4×4 크기의 보드에서 혼자 즐기는 재미있는 게임이다. 이 [링크](https://gabrielecirulli.github.io/2048/)를 누르면 게임을 해볼 수 있다.

이 게임에서 한 번의 이동은 보드 위에 있는 전체 블록을 상하좌우 네 방향 중 하나로 이동시키는 것이다. 이때, 같은 값을 갖는 두 블록이 충돌하면 두 블록은 하나로 합쳐지게 된다. 한 번의 이동에서 이미 합쳐진 블록은 또 다른 블록과 다시 합쳐질 수 없다. (실제 게임에서는 이동을 한 번 할 때마다 블록이 추가되지만, 이 문제에서 블록이 추가되는 경우는 없다)

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/1.png)
<그림 1>

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/2.png)
<그림 2>

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/3.png)
<그림 3>

<그림 1>의 경우에서 위로 블록을 이동시키면 <그림 2>의 상태가 된다. 여기서, 왼쪽으로 블록을 이동시키면 <그림 3>의 상태가 된다.

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/4.png)
<그림 4>

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/5.png)
<그림 5>

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/6.png)
<그림 6>

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/7.png)
<그림 7>

<그림 4>의 상태에서 블록을 오른쪽으로 이동시키면 <그림 5>가 되고, 여기서 다시 위로 블록을 이동시키면 <그림 6>이 된다. 여기서 오른쪽으로 블록을 이동시켜 <그림 7>을 만들 수 있다.

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/8.png)
<그림 8>

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/10.png)
<그림 9>

<그림 8>의 상태에서 왼쪽으로 블록을 옮기면 어떻게 될까? 2가 충돌하기 때문에, 4로 합쳐지게 되고 <그림 9>의 상태가 된다.

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/17.png)
<그림 10>

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/18.png)
<그림 11>

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/19.png)
<그림 12>


![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/20.png)
<그림 13>

<그림 10>에서 위로 블록을 이동시키면 <그림 11>의 상태가 된다. 

<그림 12>의 경우에 위로 블록을 이동시키면 <그림 13>의 상태가 되는데, 그 이유는 한 번의 이동에서 이미 합쳐진 블록은 또 합쳐질 수 없기 때문이다.

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/21.png)
<그림 14>

![](https://onlinejudgeimages.s3-ap-northeast-1.amazonaws.com/problem/12094/22.png)
<그림 15>

마지막으로, 똑같은 수가 세 개가 있는 경우에는 이동하려고 하는 쪽의 칸이 먼저 합쳐진다. 예를 들어, 위로 이동시키는 경우에는 위쪽에 있는 블록이 먼저 합쳐지게 된다. <그림 14>의 경우에 위로 이동하면 <그림 15>를 만든다.

이 문제에서 다루는 2048 게임은 보드의 크기가 N×N 이다. 보드의 크기와 보드판의 블록 상태가 주어졌을 때, 최대 5번 이동해서 만들 수 있는 가장 큰 블록의 값을 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 보드의 크기 N (1 ≤ N ≤ 20)이 주어진다. 둘째 줄부터 N개의 줄에는 게임판의 초기 상태가 주어진다. 0은 빈 칸을 나타내며, 이외의 값은 모두 블록을 나타낸다. 블록에 쓰여 있는 수는 2보다 크거나 같고, 1024보다 작거나 같은 2의 제곱꼴이다. 블록은 적어도 하나 주어진다.

## 출력

최대 5번 이동시켜서 얻을 수 있는 가장 큰 블록을 출력한다.

## 예제 입력 1 

3
2 2 2
4 4 4
8 8 8

## 예제 출력 1 

16

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N;
    static int[][] map;
    static int[][] copy;
    static int[] direction;
    static int[] dx = {-1, 1, 0, 0};
    static int[] dy = {0, 0, -1, 1};
    static int max = Integer.MIN_VALUE;
    static boolean[][] visit;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());

        map = new int[N][N];        // 초기 보드판
        direction = new int[5];     // 방향 저장할 배열

        for (int i = 0; i < N; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        backtracking(0);
        System.out.println(max);
    }

    public static void backtracking(int depth) {

        /**
         * 종료 조건
         * direction 배열이 가득 차면, 요소를 꺼내서 이동시키는 로직(checking -> combine)을 거쳐서 max값을 구함!
         */
        if (depth == 5) {
            checking();
            for (int i = 0; i < N; i++) {
                for (int j = 0; j < N; j++) {
                    max = Math.max(max, copy[i][j]);
                }
            }
            return;
        }

        for (int i = 0; i < 4; i++) {
            direction[depth] = i;
            backtracking(depth+1);
        }
    }

    public static void checking() {
        copy = new int[N][N];
        for (int i = 0; i < N; i++) {
            copy[i] = map[i].clone();
        }

        for (int dir : direction) {
            visit = new boolean[N][N];

            switch (dir) {
                // 상, 좌
                case 0: case 2:
                    for (int i = 0; i < N; i++) {
                        for (int j = 0; j < N; j++) {
                            combine(i, j, dir);
                        }
                    }
                    break;

                // 하, 아래부터 탐색해야 하므로 x축 인덱스는 최대에서 점점 작아지는 수가 됨
                case 1:
                    for (int i = N-1; i >= 0; i--) {
                        for (int j = 0; j < N; j++) {
                            combine(i, j, dir);
                        }
                    }
                    break;

                // 우, 오른쪽부터 탐색해야 하므로 y축 인덱스가 최대에서 점점 작아짐
                case 3:
                    for (int i = 0; i < N; i++) {
                        for (int j = N-1; j >= 0; j--) {
                            combine(i, j, dir);
                        }
                    }
                    break;
            }
        }
    }

    public static void combine(int x, int y, int dir) {

        if (copy[x][y] == 0) return;    // 빈 칸인 요소는 움직일 필요가 없다!

        while (true) {
            int cx = x + dx[dir];
            int cy = y + dy[dir];

            if (cx < 0 || cy < 0 || cx >= N || cy >= N) return;

            if (visit[cx][cy]) return;

            if (cx >= 0 && cy >= 0 && cx < N && cy < N) {
                if (!visit[cx][cy]) {

                    /**
                     * 같은 블록이면 합치고, 현재 블록을 빈 칸으로 만들어준다.
                     * 다른 블록이면 아무것도 하지 않는다!
                     * 이동하려는 블록이 0이라면 현재 블록을 이동시키고 현재 블록을 빈 칸으로 만든다.
                     * 작업의 반복을 위해 x = cx, y = cy
                     */
                    if (copy[cx][cy] == copy[x][y]) {
                        visit[cx][cy] = true;
                        copy[cx][cy] *= 2;
                        copy[x][y] = 0;
                        return;
                    } else if (copy[cx][cy] != 0) {
                        return;
                    }

                    copy[cx][cy] = copy[x][y];
                    copy[x][y] = 0;
                    x = cx;
                    y = cy;
                }
            }
        }
    }
}
```
- 게임을 직접 해보면서 규칙을 익히니까 좀 더 이해하기가 수월했다.
- 기본적으로 이동하려는 곳이 같은 블록이면 합쳐주고 현재 블록을 빈 칸으로 만들어주며,
- 이동하려는 곳이 빈 칸이면 이동하고 나와 다른 블록이면 이동하지 않는다.
- 이 때 이동 조건이 맞다면 보드판 끝까지 이동을 해야 하기 때문에 이동 로직은 while문을 이용했다
- 그리고 여러 블록이 같은 경우에는 이동하려는 방향의 블록들이 우선적으로 합쳐지기 때문에 이동 방향에 따라 탐색 방향 또한 다르게 가져가야 한다. 