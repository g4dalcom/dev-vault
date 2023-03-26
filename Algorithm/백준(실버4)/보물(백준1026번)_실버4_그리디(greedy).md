# [문제링크](https://www.acmicpc.net/problem/1026)

## 📝 문제

옛날 옛적에 수학이 항상 큰 골칫거리였던 나라가 있었다. 이 나라의 국왕 김지민은 다음과 같은 문제를 내고 큰 상금을 걸었다.

길이가 N인 정수 배열 A와 B가 있다. 다음과 같이 함수 S를 정의하자.

S = A[0] × B[0] + ... + A[N-1] × B[N-1]

S의 값을 가장 작게 만들기 위해 A의 수를 재배열하자. 단, B에 있는 수는 재배열하면 안 된다.

S의 최솟값을 출력하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N이 주어진다. 둘째 줄에는 A에 있는 N개의 수가 순서대로 주어지고, 셋째 줄에는 B에 있는 수가 순서대로 주어진다. N은 50보다 작거나 같은 자연수이고, A와 B의 각 원소는 100보다 작거나 같은 음이 아닌 정수이다.

## 출력

첫째 줄에 S의 최솟값을 출력한다.

## 예제 입력 1 

5
1 1 1 6 0
2 7 8 3 1

## 예제 출력 1 

18

## 예제 입력 2 

3
1 1 3
10 30 20

## 예제 출력 2 
80

## 예제 입력 3 

9
5 15 100 31 39 0 0 3 26
11 12 13 2 3 4 5 9 1

## 예제 출력 3 

528

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());

        Integer[] A = new Integer[N];
        Integer[] B = new Integer[N];

        // 입력
        for (int i = 0; i < 2; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            for (int j = 0 ; j < N; j++) {
                if (i == 0)
                    A[j] = Integer.parseInt(st.nextToken());
                else
                    B[j] = Integer.parseInt(st.nextToken());
            }
        }

        // A를 내림차순으로 정렬!
        Arrays.sort(A, new Comparator<Integer>() {
            @Override
            public int compare(final Integer o1, final Integer o2) {
                return o2 - o1;
            }
        });

        // 가장 큰 수부터 가장 작은 수로 만드는 조합을 만들어주기!
        int result = 0;
        boolean[] check = new boolean[N];
        for (int i = 0; i < N; i++) {
            int tmp = 0;
            int min = Integer.MAX_VALUE;
            for (int j = 0; j < N; j++) {
                if (!check[j] && min > A[i] * B[j]) {
                    tmp = j;
                    min = A[i] * B[j];
                }
            }
            check[tmp] = true;
            result += min;
        }
        System.out.println(result);
    }
}
```
- 문제에 B 배열은 재배열하지 말라는 문구가 있어서 A만 정렬한 뒤 2중 for문으로 구하였는데 다른 답안들을 보니 B도 정렬해서 구했더라..
- 사실 답을 구하는 데는 B를 정렬했는지 안 했는지 모르기 때문에 상관 없지만 그래도 코딩 테스트에서 주어진 조건이라면 B를 건드리면 안 되기 때문에 고민해본 게 나쁜 거 같지는 않다!