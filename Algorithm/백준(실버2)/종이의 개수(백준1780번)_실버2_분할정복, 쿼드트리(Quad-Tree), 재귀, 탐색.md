# [문제링크](https://www.acmicpc.net/problem/1780)

## 📝 문제

N×N크기의 행렬로 표현되는 종이가 있다. 종이의 각 칸에는 -1, 0, 1 중 하나가 저장되어 있다. 우리는 이 행렬을 다음과 같은 규칙에 따라 적절한 크기로 자르려고 한다.

1.  만약 종이가 모두 같은 수로 되어 있다면 이 종이를 그대로 사용한다.
2.  (1)이 아닌 경우에는 종이를 같은 크기의 종이 9개로 자르고, 각각의 잘린 종이에 대해서 (1)의 과정을 반복한다.

이와 같이 종이를 잘랐을 때, -1로만 채워진 종이의 개수, 0으로만 채워진 종이의 개수, 1로만 채워진 종이의 개수를 구해내는 프로그램을 작성하시오.

## 입력

첫째 줄에 N(1 ≤ N ≤ 37, N은 3k 꼴)이 주어진다. 다음 N개의 줄에는 N개의 정수로 행렬이 주어진다.

## 출력

첫째 줄에 -1로만 채워진 종이의 개수를, 둘째 줄에 0으로만 채워진 종이의 개수를, 셋째 줄에 1로만 채워진 종이의 개수를 출력한다.

## 예제 입력 1 

9
0 0 0 1 1 1 -1 -1 -1
0 0 0 1 1 1 -1 -1 -1
0 0 0 1 1 1 -1 -1 -1
1 1 1 0 0 0 0 0 0
1 1 1 0 0 0 0 0 0
1 1 1 0 0 0 0 0 0
0 1 -1 0 1 -1 0 1 -1
0 -1 1 0 1 -1 0 1 -1
0 1 -1 1 0 -1 0 1 -1

## 예제 출력 1 

10
12
11


---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {
    static int N;
    static int minus, zero, plus = 0;
    static int[][] papers;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());

        /* 입력받기 */
        papers = new int[N][N];
        for (int i = 0; i < N; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N; j++) {
                papers[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        cutting(0, 0, N);
        System.out.println(minus + "\n" + zero + "\n" + plus);
    }

    public static void cutting(int row, int col, int size) {
        /**
         * 첫 요소를 기준으로 종이 내에 모든 요소가 같은지를 체크한다.
         * 모두 같다면 해당 요소 변수를 카운팅해준다!
         * 같지 않다면 종이를 가로세로 3등분하고 9개로 나누어진 구역들을 각각 cutting 메서드로 부른다!
         */
        if (isSameNumber(row, col, size)) {
            if (papers[row][col] == -1) minus++;
            else if (papers[row][col] == 0) zero++;
            else plus++;

        } else {

            int cuttingSize = size / 3;

            cutting(row, col, cuttingSize);
            cutting(row, col+cuttingSize, cuttingSize);
            cutting(row, col+cuttingSize*2, cuttingSize);
            cutting(row+cuttingSize, col, cuttingSize);
            cutting(row+cuttingSize, col+cuttingSize, cuttingSize);
            cutting(row+cuttingSize, col+cuttingSize*2, cuttingSize);
            cutting(row+cuttingSize*2, col, cuttingSize);
            cutting(row+cuttingSize*2, col+cuttingSize, cuttingSize);
            cutting(row+cuttingSize*2, col+cuttingSize*2, cuttingSize);

        }
    }

    public static boolean isSameNumber(int row, int col, int size) {
        /* 첫번째 요소를 기준으로 체크 */
        int Num = papers[row][col];

        for (int i = row; i < row+size; i++) {
            for (int j = col; j < col+size; j++) {
                if (papers[i][j] != Num) {
                    return false;
                }
            }
        }
        return true;
    }
}
```
- 며칠 전에 풀었던 색종이 문제와 거의 비슷한 문제였다! 그 때는 답을 보고 이해한 후에 코드화하면서도 계속 답안을 참고하면서 완성하였었는데 이번에는 혼자 힘으로 풀어내서 매우 뿌듯하다!
