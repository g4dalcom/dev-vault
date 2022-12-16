# [문제링크](https://www.acmicpc.net/problem/6603)

## 📝 문제

독일 로또는 {1, 2, ..., 49}에서 수 6개를 고른다.

로또 번호를 선택하는데 사용되는 가장 유명한 전략은 49가지 수 중 k(k>6)개의 수를 골라 집합 S를 만든 다음 그 수만 가지고 번호를 선택하는 것이다.

예를 들어, k=8, S={1,2,3,5,8,13,21,34}인 경우 이 집합 S에서 수를 고를 수 있는 경우의 수는 총 28가지이다. ([1,2,3,5,8,13], [1,2,3,5,8,21], [1,2,3,5,8,34], [1,2,3,5,13,21], ..., [3,5,8,13,21,34])

집합 S와 k가 주어졌을 때, 수를 고르는 모든 방법을 구하는 프로그램을 작성하시오.

## 입력

입력은 여러 개의 테스트 케이스로 이루어져 있다. 각 테스트 케이스는 한 줄로 이루어져 있다. 첫 번째 수는 k (6 < k < 13)이고, 다음 k개 수는 집합 S에 포함되는 수이다. S의 원소는 오름차순으로 주어진다.

입력의 마지막 줄에는 0이 하나 주어진다. 

## 출력

각 테스트 케이스마다 수를 고르는 모든 방법을 출력한다. 이때, 사전 순으로 출력한다.

각 테스트 케이스 사이에는 빈 줄을 하나 출력한다.

## 예제 입력 1 

7 1 2 3 4 5 6 7
8 1 2 3 5 8 13 21 34
0

## 예제 출력 1

1 2 3 4 5 6
1 2 3 4 5 7
1 2 3 4 6 7
1 2 3 5 6 7
1 2 4 5 6 7
1 3 4 5 6 7
2 3 4 5 6 7

1 2 3 5 8 13
1 2 3 5 8 21
1 2 3 5 8 34
1 2 3 5 13 21
1 2 3 5 13 34
1 2 3 5 21 34
1 2 3 8 13 21
1 2 3 8 13 34
1 2 3 8 21 34
1 2 3 13 21 34
1 2 5 8 13 21
1 2 5 8 13 34
1 2 5 8 21 34
1 2 5 13 21 34
1 2 8 13 21 34
1 3 5 8 13 21
1 3 5 8 13 34
1 3 5 8 21 34
1 3 5 13 21 34
1 3 8 13 21 34
1 5 8 13 21 34
2 3 5 8 13 21
2 3 5 8 13 34
2 3 5 8 21 34
2 3 5 13 21 34
2 3 8 13 21 34
2 5 8 13 21 34
3 5 8 13 21 34


---

### 🔍 정답

```java
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
import java.util.*;  
  
public class Main {  
    static StringBuilder sb = new StringBuilder();  
    static int[] input, output;  
    static boolean[] visit;  
    static int k;  
  
    public static void main(String[] args) throws IOException {  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
  
        while (true) {  
            StringTokenizer st = new StringTokenizer(br.readLine());  
            k = Integer.parseInt(st.nextToken());  
            if (k == 0) {  
                System.out.println(sb);  
                break;  
            }  
  
            input = new int[k]; // 입력값 담을 배열  
            output = new int[6]; // 출력값 담을 배열  
            visit = new boolean[k]; // 중복을 배제할 방문 체크 배열  
  
            for (int i = 0; i < k; i++) {  
                input[i] = Integer.parseInt(st.nextToken());  
            }  
            /**  
             * idx를 파라미터로 주는 이유!!  
             * 순열이 아니라 "조합" 이므로  
             * [1, 2, 3] 과 [1, 3, 2] 처럼 순서만 다른 배열은 같은 것으로 보고 배제해야 하기 때문에  
             * 현재 선택된 값보다 뒤에 있는 값만 탐색하면 된다.  
             */            dfs(0, 0);  
            sb.append("\n");  
        }  
    }  
  
    public static void dfs(int idx, int depth) {  
        if (depth == 6) {  
            for (int var : output) {  
                sb.append(var).append(" ");  
            }  
            sb.append("\n");  
            return;  
        }  
  
        for (int i = idx; i < k; i++) {  
            if (!visit[i]) {  
                visit[i] = true;  
                output[depth] = input[i];  
                dfs(i, depth + 1);  
                visit[i] = false;  
            }  
        }  
    }  
}
```

