# [문제링크](https://www.acmicpc.net/problem/1072)

## 📝 문제

김형택은 지금 몰래 Spider Solitaire(스파이더 카드놀이)를 하고 있다. 형택이는 이 게임을 이길 때도 있었지만, 질 때도 있었다. 누군가의 시선이 느껴진 형택이는 게임을 중단하고 코딩을 하기 시작했다. 의심을 피했다고 생각한 형택이는 다시 게임을 켰다. 그 때 형택이는 잠시 코딩을 하는 사이에 자신의 게임 실력이 눈에 띄게 향상된 것을 알았다.

이제 형택이는 앞으로의 모든 게임에서 지지 않는다. 하지만, 형택이는 게임 기록을 삭제 할 수 없기 때문에, 자신의 못하던 예전 기록이 현재 자신의 엄청난 실력을 증명하지 못한다고 생각했다.

게임 기록은 다음과 같이 생겼다.

-   게임 횟수 : X
-   이긴 게임 : Y (Z%)
-   Z는 형택이의 승률이고, 소수점은 버린다. 예를 들어, X=53, Y=47이라면, Z=88이다.

X와 Y가 주어졌을 때, 형택이가 게임을 최소 몇 번 더 해야 Z가 변하는지 구하는 프로그램을 작성하시오.

## 입력

각 줄에 정수 X와 Y가 주어진다.

## 출력

첫째 줄에 형택이가 게임을 최소 몇 판 더 해야하는지 출력한다. 만약 Z가 절대 변하지 않는다면 -1을 출력한다.

## 제한

-   1 ≤ X ≤ 1,000,000,000
-   0 ≤ Y ≤ X

## 예제 입력 1 

10 8

## 예제 출력 1 

1

## 예제 입력 2 

100 80

## 예제 출력 2 

6

## 예제 입력 3
47 47

## 예제 출력 3 
-1

## 예제 입력 4

99000 0

## 예제 출력 4 

1000

## 예제 입력 5 

1000000000 470000000

## 예제 출력 5 

19230770

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int X = Integer.parseInt(st.nextToken());
        int Y = Integer.parseInt(st.nextToken());
        
        // Y에 100을 먼저 곱해주어야 계산이 수월한데 이 경우 int 범위를 벗어날 수 있으므로 long타입을 사용함!
        long Z = Y * 100L / X;

        // 소수점을 버림하기 때문에 애초에 승률이 99프로라면 변할 수가 없다. (한 번이라도 졌다면 100프로가 될 수 없으므로!)
        if (X == Y || Z >= 99) {
            System.out.println(-1);
            return;
        }

        int lo = 0;
        int hi = X;

        // Upper bound
        while (lo < hi) {
            int mid = (lo + hi) / 2;

            long calc = (Y+mid) * 100L / (X+mid);

            if (calc <= Z) {
                lo = mid + 1;
            } else {
                hi = mid;
            }
        }

        System.out.println(lo);
    }
}
```
- 소수점 버림을 하기 때문에 확률이 99프로 이상이라면 아무리 이겨도 확률이 변할 수가 없다. 이 점을 고려해서 -1을 출력하는 조건을 잡아주어야 하고
- 이분 탐색의 대상이 되는 요소는 변화를 추적해야 하는 값, 승률이 된다. 따라서 초깃값 Z와 게임 수(mid)를 더해서 얻어지는 calc 값을 비교하며 이분 탐색을 하게 된다.
- 그리고 Z = 80일 때, 81이 되는 순간을 구해야 하는 것이므로 target값을 초과하는 순간의 값을 반환해주는 upper bound 알고리즘을 이용하였다!
- hi값을 구하는 게 고민이 되었는데 특정 정수를 넣긴 어려웠고 승률이 0프로 일 때, 지금까지 한 게임수 만큼을 더 한다면 승률이 50프로가 된다는 것이 보장되기 때문에 X값을 넣어서 구하였다!