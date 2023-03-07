# [문제링크](https://www.acmicpc.net/problem/2580)

## 📝 문제

스도쿠는 18세기 스위스 수학자가 만든 '라틴 사각형'이랑 퍼즐에서 유래한 것으로 현재 많은 인기를 누리고 있다. 이 게임은 아래 그림과 같이 가로, 세로 각각 9개씩 총 81개의 작은 칸으로 이루어진 정사각형 판 위에서 이뤄지는데, 게임 시작 전 일부 칸에는 1부터 9까지의 숫자 중 하나가 쓰여 있다.

![](https://upload.acmicpc.net/508363ac-0289-4a92-a639-427b10d66633/-/preview/)

나머지 빈 칸을 채우는 방식은 다음과 같다.

1.  각각의 가로줄과 세로줄에는 1부터 9까지의 숫자가 한 번씩만 나타나야 한다.
2.  굵은 선으로 구분되어 있는 3x3 정사각형 안에도 1부터 9까지의 숫자가 한 번씩만 나타나야 한다.

위의 예의 경우, 첫째 줄에는 1을 제외한 나머지 2부터 9까지의 숫자들이 이미 나타나 있으므로 첫째 줄 빈칸에는 1이 들어가야 한다.

![](https://upload.acmicpc.net/38e505c6-0452-4a56-b01c-760c85c6909b/-/preview/)

또한 위쪽 가운데 위치한 3x3 정사각형의 경우에는 3을 제외한 나머지 숫자들이 이미 쓰여있으므로 가운데 빈 칸에는 3이 들어가야 한다.

![](https://upload.acmicpc.net/89873d9d-56ae-44f7-adb2-bd5d7e243016/-/preview/)

이와 같이 빈 칸을 차례로 채워 가면 다음과 같은 최종 결과를 얻을 수 있다.

![](https://upload.acmicpc.net/fe68d938-770d-46ea-af71-a81076bc3963/-/preview/)

게임 시작 전 스도쿠 판에 쓰여 있는 숫자들의 정보가 주어질 때 모든 빈 칸이 채워진 최종 모습을 출력하는 프로그램을 작성하시오.

## 입력

아홉 줄에 걸쳐 한 줄에 9개씩 게임 시작 전 스도쿠판 각 줄에 쓰여 있는 숫자가 한 칸씩 띄워서 차례로 주어진다. 스도쿠 판의 빈 칸의 경우에는 0이 주어진다. 스도쿠 판을 규칙대로 채울 수 없는 경우의 입력은 주어지지 않는다.

## 출력

모든 빈 칸이 채워진 스도쿠 판의 최종 모습을 아홉 줄에 걸쳐 한 줄에 9개씩 한 칸씩 띄워서 출력한다.

스도쿠 판을 채우는 방법이 여럿인 경우는 그 중 하나만을 출력한다.

## 제한

-   12095번 문제에 있는 소스로 풀 수 있는 입력만 주어진다.
    -   C++14: 80ms
    -   Java: 292ms
    -   PyPy3: 1172ms

## 예제 입력 1 

0 3 5 4 6 9 2 7 8
7 8 2 1 0 5 6 0 9
0 6 0 2 7 8 1 3 5
3 2 1 0 4 6 8 9 7
8 0 4 9 1 3 5 0 6
5 9 6 8 2 0 4 1 3
9 1 7 6 5 2 0 8 0
6 0 3 7 0 1 9 5 2
2 5 8 3 9 4 7 6 0

## 예제 출력 1 

1 3 5 4 6 9 2 7 8
7 8 2 1 3 5 6 4 9
4 6 9 2 7 8 1 3 5
3 2 1 5 4 6 8 9 7
8 7 4 9 1 3 5 2 6
5 9 6 8 2 7 4 1 3
9 1 7 6 5 2 3 8 4
6 4 3 7 8 1 9 5 2
2 5 8 3 9 4 7 6 1

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.StringTokenizer;

public class Main {
    static int[][] board;
    static ArrayList<Location> empty;
    static StringBuilder sb = new StringBuilder();

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        board = new int[9][9];          // 스도쿠 보드
        empty = new ArrayList<>();      // 빈 칸의 좌표를 담을 리스트

        // 스도쿠 보드를 입력받고 빈 칸(0)은 따로 리스트에 저장!
        for (int i = 0; i < 9; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            for (int j = 0; j < 9; j++) {
                board[i][j] = Integer.parseInt(st.nextToken());

                if (board[i][j] == 0) empty.add(new Location(i, j));
            }
        }

        sudoku(0);
    }

    public static void sudoku(int list) {

        // 리스트를 모두 순회하면 출력을 하고, 하나의 경우의 수만 출력하면 되므로 종료까지 해준다!
        if (list == empty.size()) {
            for (int i = 0; i < 9; i++) {
                for (int j = 0; j < 9; j++) {
                    sb.append(board[i][j]).append(" ");
                }
                sb.append("\n");
            }
            System.out.println(sb);
            System.exit(0);
        }

        /** 
         * 리스트에서 좌표를 꺼낸 후 행과 열, 3*3칸을 1~9까지의 수를 하나씩 넣어보면서 겹치지 않는 수를 넣어준다!
         */
        Location lo = empty.get(list);
        for (int i = 1; i <= 9; i++) {
            if (check(lo.x, lo.y, i)) {
                board[lo.x][lo.y] = i;
                sudoku(list + 1);
                board[lo.x][lo.y] = 0;
            }
        }
    }

    public static boolean check(int x, int y, int num) {

        // 같은 행(board[x][i])과 같은 열(board[i][y])에서 겹치는 수가 있는지 확인
        for (int i = 0; i < 9; i++) {
            if (board[x][i] == num)
                return false;
            if (board[i][y] == num)
                return false;
        }

        /**
         * 자신이 속한 네모칸에서 겹치는 수가 있는지 확인
         */
        int cx = (x / 3) * 3;
        int cy = (y / 3) * 3;
        for (int i = cx; i < cx + 3; i++) {
            for (int j = cy; j < cy + 3; j++) {
                if (board[i][j] == num)
                    return false;
            }
        }

        return true;
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
- 빈 칸을 돌면서 빈 칸이 속하는 행과 열, 그리고 3x3 네모칸 모두에서 겹치지 않는 수를 찾아서 넣어주는 방식으로 풀어보았다.
- 네모칸에서 겹치지 않는 수를 찾을 때, 첫 번째 칸에 속하는 좌표들은 모두 (0, 0) ~ (2, 2)의 범위를 체크하여야 한다. 따라서 (0, 0)이든 (1, 2)이든 (2, 2)든 초깃값을 (0, 0)으로 만들어주어야 하고 가장 마지막 칸은 (6, 6)을 초깃값으로 만들어주어야 정상적으로 for문을 통해 체크할 수가 있다.
- 우선 가능한 초깃값을 나열해보면 (0, 0), (0, 3), (0, 6), (3, 0), (3, 3) ... 이런 식으로 단위가 3씩 차이가 나는 것을 알 수 있다.
- 따라서 전체 보드판에서 행과 열이 3분할 되어있으므로 3으로 나누어준 후 x3 을 하면 초깃값을 구할 수 있다!