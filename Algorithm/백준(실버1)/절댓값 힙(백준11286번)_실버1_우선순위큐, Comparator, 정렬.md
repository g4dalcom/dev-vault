# [문제링크](https://www.acmicpc.net/problem/11286)

## 📝 문제

절댓값 힙은 다음과 같은 연산을 지원하는 자료구조이다.

1.  배열에 정수 x (x ≠ 0)를 넣는다.
2.  배열에서 절댓값이 가장 작은 값을 출력하고, 그 값을 배열에서 제거한다. 절댓값이 가장 작은 값이 여러개일 때는, 가장 작은 수를 출력하고, 그 값을 배열에서 제거한다.

프로그램은 처음에 비어있는 배열에서 시작하게 된다.

## 입력

첫째 줄에 연산의 개수 N(1≤N≤100,000)이 주어진다. 다음 N개의 줄에는 연산에 대한 정보를 나타내는 정수 x가 주어진다. 만약 x가 0이 아니라면 배열에 x라는 값을 넣는(추가하는) 연산이고, x가 0이라면 배열에서 절댓값이 가장 작은 값을 출력하고 그 값을 배열에서 제거하는 경우이다. 입력되는 정수는 -231보다 크고, 231보다 작다.

## 출력

입력에서 0이 주어진 회수만큼 답을 출력한다. 만약 배열이 비어 있는 경우인데 절댓값이 가장 작은 값을 출력하라고 한 경우에는 0을 출력하면 된다.

## 예제 입력 1

	18
1
-1
0
0
0
1
1
-1
-1
2
-2
0
0
0
0
0
0
0

## 예제 출력 1

-1
1
0
-1
-1
1
1
-2
2
0

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

import static java.lang.Math.abs;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int x = Integer.parseInt(br.readLine());

        PriorityQueue<Integer> pq = new PriorityQueue<>(new Comparator<Integer>() {

            /**
             * Comparator
             * 양수를 반환하면 자리를 바꾸고 음수를 반환하면 기존 자리를 유지한다.
             * @if문 먼저 들어온 o1 값이 더 크면 뒤로 보내야 하므로 1을 반환, 더 작다면 음수를 반환해서 자리를 유지한다.
             * @else문 결과가 양수라면 abs(o1)이 더 크므로 자리를 바꾸어야 하고 음수라면 o2가 더 크므로 자리를 유지한다.
             */
            @Override
            public int compare(final Integer o1, final Integer o2) {
                if (abs(o1) == abs(o2)) return o1 > o2 ? 1 : -1;        // 절댓값이 같은 경우, 더 작은 값을 앞으로 보낸다.
                else return abs(o1) - abs(o2);                          // 절댓값이 다른 경우, 절댓값이 더 작은 순서로 정렬
            }
        });

        for (int i = 0 ; i < x; i++) {
            int input = Integer.parseInt(br.readLine());
            if (input == 0) {
                if (pq.isEmpty()) System.out.println(0);
                else System.out.println(pq.poll());
            }
            else pq.offer(input);
        }
    }
}
```
- 이전에 최소힙이라는 문제와 유사해서 우선순위 큐를 사용해야겠다는 생각을  했다.
- 그러나 이번 문제에서는 절댓값이라는 기준이 하나 더 생겼기 때문에 Comparator 를 오버라이딩해서 해결하였다!