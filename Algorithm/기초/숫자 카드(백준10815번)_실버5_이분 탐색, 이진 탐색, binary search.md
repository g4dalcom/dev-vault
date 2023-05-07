# [문제링크](https://www.acmicpc.net/problem/10815)

## 📝 문제

숫자 카드는 정수 하나가 적혀져 있는 카드이다. 상근이는 숫자 카드 N개를 가지고 있다. 정수 M개가 주어졌을 때, 이 수가 적혀있는 숫자 카드를 상근이가 가지고 있는지 아닌지를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 상근이가 가지고 있는 숫자 카드의 개수 N(1 ≤ N ≤ 500,000)이 주어진다. 둘째 줄에는 숫자 카드에 적혀있는 정수가 주어진다. 숫자 카드에 적혀있는 수는 -10,000,000보다 크거나 같고, 10,000,000보다 작거나 같다. 두 숫자 카드에 같은 수가 적혀있는 경우는 없다.

셋째 줄에는 M(1 ≤ M ≤ 500,000)이 주어진다. 넷째 줄에는 상근이가 가지고 있는 숫자 카드인지 아닌지를 구해야 할 M개의 정수가 주어지며, 이 수는 공백으로 구분되어져 있다. 이 수도 -10,000,000보다 크거나 같고, 10,000,000보다 작거나 같다

## 출력

첫째 줄에 입력으로 주어진 M개의 수에 대해서, 각 수가 적힌 숫자 카드를 상근이가 가지고 있으면 1을, 아니면 0을 공백으로 구분해 출력한다.

## 예제 입력 1 

5
6 3 2 10 -10
8
10 9 -5 2 3 4 5 -10

## 예제 출력 1 

1 0 0 1 1 0 0 1

---

### 🔍 정답

```java
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
import java.util.Arrays;  
import java.util.StringTokenizer;  
  
public class Main {  
    static int[] card;  
    static int N;  
  
    public static void main(String[] args) throws IOException {  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        N = Integer.parseInt(br.readLine());  
  
        StringTokenizer st = new StringTokenizer(br.readLine());  
  
        card = new int[N];  
        for (int i = 0; i < N; i++) {  
            card[i] = Integer.parseInt(st.nextToken());  
        }  
        Arrays.sort(card);  
  
        int M = Integer.parseInt(br.readLine());  
  
        st = new StringTokenizer(br.readLine());  
        StringBuilder sb = new StringBuilder();  
  
        for (int i = 0; i < M; i++) {  
            int target = Integer.parseInt(st.nextToken());  
            sb.append(bs(target)).append(" ");  
        }  
  
        System.out.println(sb);  
    }  
  
    public static int bs(int target) {  
        int lo = 0;  
        int hi = N-1;  
  
        while (lo <= hi) {  
            int mid = lo + ((hi - lo) / 2);  
  
            if (target == card[mid]) return 1;  
  
            if (target < card[mid]) {  
                hi = mid - 1;  
            } else {  
                lo = mid + 1;  
            }  
        }  
  
        return 0;  
    }  
}
```
- 범위가 아니라 특정 수가 있는지 여부만을 따지면 되기 때문에 일반적인 이분 탐색 알고리즘으로 접근하면 된다!