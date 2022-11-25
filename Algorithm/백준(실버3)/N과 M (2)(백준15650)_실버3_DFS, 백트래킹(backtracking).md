# [문제링크](https://www.acmicpc.net/problem/15650)

## 📝 문제

자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

-   1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열
-   고른 수열은 오름차순이어야 한다.

## 입력

첫째 줄에 자연수 N과 M이 주어진다. (1 ≤ M ≤ N ≤ 8)

## 출력

한 줄에 하나씩 문제의 조건을 만족하는 수열을 출력한다. 중복되는 수열을 여러 번 출력하면 안되며, 각 수열은 공백으로 구분해서 출력해야 한다.

수열은 사전 순으로 증가하는 순서로 출력해야 한다.

## 예제 입력 1 

3 1

## 예제 출력 1 

1
2
3

## 예제 입력 2 

4 2

## 예제 출력 2 

1 2
1 3
1 4
2 3
2 4
3 4

## 예제 입력 3 

4 4

## 예제 출력 3 

1 2 3 4


---

### 🔍 나의 정답

```java
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
import java.util.StringTokenizer;  
  
public class Main {  
    public static int N;  
    public static int M;  
    public static int[] array;  
    public static boolean[] visit;  
    public static StringBuilder sb = new StringBuilder();  
  
    public static void main(String[] args) throws IOException {  
  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        StringTokenizer st = new StringTokenizer(br.readLine());  
        N = Integer.parseInt(st.nextToken());  
        M = Integer.parseInt(st.nextToken());  
  
        array = new int[M];  
        visit = new boolean[N];  
        dfs(0);  
        System.out.println(sb);  
    }  
  
    public static void dfs(int depth) {  
        if (depth == M) {  

			/* 추가된 부분 */ 
            int max = array[0];  
            for (int i = 1; i < M; i++) {  
                if (max < array[i]) max = array[i];  
                else return;  
            }  
  
            for (int var : array) {  
                sb.append(var).append(' ');  
            }  
            sb.append("\n");  
            return;  
        }  
  
        for (int i = 0; i < N; i++) {  
            if (!visit[i]) {  
                visit[i] = true;  
                array[depth] = i + 1;  
                dfs(depth + 1);  
                visit[i] = false;  
            }  
        }  
    }  
}
```
- [[N과 M (1)(백준15649번)_실버3_DFS, 백트래킹(backtracking)]]
- N과 M (1)에서 array에 있는 수가 오름차순일 때만 sb에 저장할 수 있도록 조건문만 추가하였다.
- 하지만 문제를 풀고 다른 사람의 풀이를 확인한 결과, 이 문제의 의도는 순열과 조합의 차이를 공부하라는 것임을 알게되었다

### 🔍 조합을 이용한 정답

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.StringTokenizer;
import java.io.IOException;
 
public class Main {
 
	public static int[] arr;
	public static int N, M;
	public static StringBuilder sb = new StringBuilder();
 
	public static void main(String[] args) throws IOException {
 
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine());
 
		N = Integer.parseInt(st.nextToken());
		M = Integer.parseInt(st.nextToken());
 
		arr = new int[M];
        
		dfs(1, 0);
		System.out.println(sb);
 
	}
 
	public static void dfs(int at, int depth) {
 
		if (depth == M) {
			for (int val : arr) {
				sb.append(val).append(' ');
			}
			sb.append('\n');
			return;
		}
        
		for (int i = at; i <= N; i++) {
 
			arr[depth] = i;
			dfs(i + 1, depth + 1);
 
		}
	}
}
```

