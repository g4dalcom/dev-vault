# [문제링크](https://www.acmicpc.net/problem/2110)

## 📝 문제

도현이의 집 N개가 수직선 위에 있다. 각각의 집의 좌표는 x1, ..., xN이고, 집 여러개가 같은 좌표를 가지는 일은 없다.

도현이는 언제 어디서나 와이파이를 즐기기 위해서 집에 공유기 C개를 설치하려고 한다. 최대한 많은 곳에서 와이파이를 사용하려고 하기 때문에, 한 집에는 공유기를 하나만 설치할 수 있고, 가장 인접한 두 공유기 사이의 거리를 가능한 크게 하여 설치하려고 한다.

C개의 공유기를 N개의 집에 적당히 설치해서, 가장 인접한 두 공유기 사이의 거리를 최대로 하는 프로그램을 작성하시오.

## 입력

첫째 줄에 집의 개수 N (2 ≤ N ≤ 200,000)과 공유기의 개수 C (2 ≤ C ≤ N)이 하나 이상의 빈 칸을 사이에 두고 주어진다. 둘째 줄부터 N개의 줄에는 집의 좌표를 나타내는 xi (0 ≤ xi ≤ 1,000,000,000)가 한 줄에 하나씩 주어진다.

## 출력

첫째 줄에 가장 인접한 두 공유기 사이의 최대 거리를 출력한다.

## 예제 입력 1 

5 3
1
2
8
4
9

## 예제 출력 1 

3

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {
    static int N, C;
    static int[] house;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());   // 집의 개수
        C = Integer.parseInt(st.nextToken());   // 공유기의 개수

        house = new int[N];

        for (int i = 0; i < N; i++) {
            house[i] = Integer.parseInt(br.readLine());
        }
        Arrays.sort(house);

        int low = 1;                            // 가능한 최소 거리
        int high = house[N-1] - house[0] + 1;   // 가능한 최대 거리, 상한 탐색은 찾는 값을 초과하는 첫 번째 인덱스를 반환하므로!

        while (low < high) {

            int mid = (low+high) / 2;

            if (bs(mid) < C) {
                high = mid;
            } else {
                low = mid+1;
            }
        }

        // 상한 탐색이니까 -1!!!
        System.out.println(low-1);
    }

    public static int bs(int distance) {
        /**
         * 첫번째 집은 설치하고 본다.
         * 첫번째 집을 설치하지 않을 경우 그래프가 그만큼 작아지기 때문에 최댓값이 나올 수가 없다.
         */
        int cnt = 1;
        int curLocate = house[0];

        for (int i = 1; i < N; i++) {
            if (house[i] - curLocate >= distance) {
                cnt++;
                curLocate = house[i];
            }
        }
        return cnt;
    }
}
```
- 이분탐색은 정말 생각하기도 어렵고 구현도 어려운 것 같다 ㅠㅠ 풀었던 문제들을 심심할 때마다 보면서 익숙해져야 할듯하다 ㅠㅠ
