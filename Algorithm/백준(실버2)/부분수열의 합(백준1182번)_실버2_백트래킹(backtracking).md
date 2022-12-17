# [문제링크](https://www.acmicpc.net/problem/1182)

## 📝 문제

N개의 정수로 이루어진 수열이 있을 때, 크기가 양수인 부분수열 중에서 그 수열의 원소를 다 더한 값이 S가 되는 경우의 수를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 정수의 개수를 나타내는 N과 정수 S가 주어진다. (1 ≤ N ≤ 20, |S| ≤ 1,000,000) 둘째 줄에 N개의 정수가 빈 칸을 사이에 두고 주어진다. 주어지는 정수의 절댓값은 100,000을 넘지 않는다.

## 출력

첫째 줄에 합이 S가 되는 부분수열의 개수를 출력한다.

## 예제 입력 1

5 0
-7 -3 -2 5 8

## 예제 출력 1 

1


---

### 🔍 정답

```java
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
import java.util.*;  
  
public class Main {  
    static int N, S;  
    static int cnt = 0;  
    static int[] arr;  
  
    public static void main(String[] args) throws IOException {  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        StringTokenizer st = new StringTokenizer(br.readLine());  
        N = Integer.parseInt(st.nextToken());  
        S = Integer.parseInt(st.nextToken());  
  
        arr = new int[N];  
        st = new StringTokenizer(br.readLine());  
        for (int i = 0; i < N; i++) {  
            arr[i] = Integer.parseInt(st.nextToken());  
        }  
  
        dfs(0, 0);  
        // 모든 수를 더하지 않은 경우(공집합)는 해당 문제에서 제외해야 하기 때문에 S == 0 일 경우 cnt를 -1 해준다!
        if (S == 0) System.out.println(cnt-1);  
        else System.out.println(cnt);  
    }  
  
    public static void dfs(int depth, int sum) {  
        if (depth == N) {  
            if (sum == S) cnt++;  
            return;  
        }  
        dfs(depth+1, sum + arr[depth]);  
        dfs(depth+1, sum);  
    }  
}
```
- 부분수열이라는 말 때문에 어려웠지만 결국 묻는 것은 요소들을 더해서 S가 되는 경우의 수를 구하라는 것이었다.
- [1, 2, 3] 이라는 배열을 예로 들면,
	- 0, 1(1), 3(1+2), 4(1+3), 2(2), 5(2+5), 3(3), 6(1+2+3) 이 경우의 수가 된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FApSL3%2FbtrTT7RvYgg%2FLd7Ssbu8a4Sy1qlHypXGz1%2Fimg.jpg)
- 보라색 숫자를 인덱스라고 생각하고,
- 각 갈래에서 왼쪽길은 해당 인덱스를 더하지 않은 값이고 오른쪽길은 해당 인덱스를 더한 값이다.
- 요약하면, `현재 인덱스를 더한 값`과 `더하지 않은 값`을 모두 탐색을 하면서 S와 비교를 하면 되는 것이다.
- 이해하기가 너무 어려워서 쉬운 예제값을 넣고 로그를 찍어보면서 따라가보았다. 예제는 [1, 2, 3] 으로 진행하였다.
- 순서는 그림 오른쪽에서 왼쪽으로 진행이 된다.
- depth = 0, sum = 0
	- 처음에 depth+1 (1)하며, 1번 인덱스를 더하면서 재귀함수를 부른다 (1)
		- depth+1 (2)하며, 2번 인덱스를 더하면서 재귀함수를 부른다 (1+2=3)
			- depth+1 (3)하며, 3번 인덱스를 더하면서 재귀함수를 부른다. (1+2+3=6)
			- S와 값을 비교하고 return!
		- depth = 2 로 돌아왔고
			- depth+1 (3) 하며, 3번 인덱스를 더하지 않고 재귀함수를 부른다. (1+2=3)
			- S와 값을 비교하고 return!
		- depth = 2 에서 비교가 끝났으므로 return!
	- depth = 1, 1번 인덱스만 더한 상태로 돌아온다. (1)
		- depth + 1 (2) 하며, 2번 인덱스를 더하지 않는다 (1)
			- depth + 1 (3) 하며, 3번 인덱스를 더한다. (1+3=4)
			- S와 값을 비교하고 return!
		- depth = 2에서,
			- depth + 1 (3) 하며, 3번 인덱스를 더하지 않는다. (1)
			- S와 값을 비교하고 return!
	- depth = 1로 돌아와서  1번 인덱스를 더하지 않는다. (0)
		- depth+1 (2) 하면서 2번 인덱스를 더한다. (2)
			- depth+1 (3) 하면서 3번 인덱스를 더한다. (2+3=5)
			- S와 값을 비교하고 return
		- depth = 2로 돌아와서, 
			- depth+1 (3) 하면서 3번 인덱스를 더하지 않는다. (2)
			- S와 값을 비교하고 return
	- depth = 1, 1번 인덱스를 더하지 않은 상태 (0)
		- depth +1 (2) 하면서 1번 인덱스를 더하지 않는다. (0)
			- depth +1 (3) 하면서 3번 인덱스를 더하고 (3)
			- S와 값을 비교하고 return
		- depth = 2, 1번 인덱스를 더하지 않은 상태 (0)
			- depth +1 (3) 하면서 3번 인덱스를 더하지 않는다 (0)
			- S와 값을 비교하고 return
- 모든 프로세스 종료!