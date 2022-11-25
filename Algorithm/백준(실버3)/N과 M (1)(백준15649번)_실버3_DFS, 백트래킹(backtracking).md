# [문제링크](https://www.acmicpc.net/problem/15649)

## 📝 문제

자연수 N과 M이 주어졌을 때, 아래 조건을 만족하는 길이가 M인 수열을 모두 구하는 프로그램을 작성하시오.

-   1부터 N까지 자연수 중에서 중복 없이 M개를 고른 수열

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
2 1
2 3
2 4
3 1
3 2
3 4
4 1
4 2
4 3

## 예제 입력 3 

4 4

## 예제 출력 3 

1 2 3 4
1 2 4 3
1 3 2 4
1 3 4 2
1 4 2 3
1 4 3 2
2 1 3 4
2 1 4 3
2 3 1 4
2 3 4 1
2 4 1 3
2 4 3 1
3 1 2 4
3 1 4 2
3 2 1 4
3 2 4 1
3 4 1 2
3 4 2 1
4 1 2 3
4 1 3 2
4 2 1 3
4 2 3 1
4 3 1 2
4 3 2 1


---

### 🔍 정답

```java
import java.io.BufferedReader;  
import java.io.IOException;  
import java.io.InputStreamReader;  
import java.util.StringTokenizer;  
  
public class Main {  
  
    public static int[] arr;  
    public static boolean[] visit;  
    public static int N;  
    public static int M;  
  
    public static StringBuilder sb = new StringBuilder();  
    public static void main(String[] args) throws IOException {  
  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
  
        StringTokenizer st = new StringTokenizer(br.readLine());  
  
        N = Integer.parseInt(st.nextToken());  
        M = Integer.parseInt(st.nextToken());  
  
        arr = new int[M];  
        visit = new boolean[N+1];  
        dfs(0);  
        System.out.println(sb);  
  
    }  

    public static void dfs(int depth) {  
        if (depth == M) {  
            for (int val : arr) {  
                sb.append(val).append(' ');  
            }  
            sb.append('\n');  
            return;  
        }  

        for (int i = 1; i <= N; i++) {  
            if (!visit[i]) {  
                visit[i] = true;  
                arr[depth] = i;  
                dfs(depth + 1);  
                visit[i] = false;  
            }  
        }  
    }  
}
```
- 기본 백트래킹 문제로 dfs를 이용해 풀어야 하는 문제이다.
- 풀이를 봐도봐도 이해가 안 가서 블로그 글은 대부분 다 본 것 같고 유튜브영상도 보고 직접 손으로 그려보며 겨우 어렴풋이 이해한 것 같다.
	- [유튜브 참고자료](https://www.youtube.com/watch?v=Enz2csssTCs&t=684s)
- 내용을 복습할 겸 정리해보려고 한다 ㅠㅠ
- 정리할 때 사용하는 예시의 내용은 N=4, M=2로 정하고 진행하기로 한다.
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4oGNT%2FbtrRX1mdVW7%2FYxISK97R1hXoQfoXYg5Nak%2Fimg.jpg)
- 기본 틀은 이러한 트리구조이다
- dfs(0) : i = 1, depth = 0
- dfs(1) : arr[0] = 1 부터 시작
- dfs(2) : arr[1]을 2로 채우고 리턴(dfs(1)), 3으로 채우고 리턴(dfs(1)), 4로 채우고 리턴(dfs(1))
- 모든 수를 돌았으면 depth 1 에서 depth 0으로 돌아가서 i = 2 로 위 내용을 반복한다

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FPhmmB%2FbtrRXD0fEvk%2FX4wPepKjboUjF26CXOlKL1%2Fimg.jpg)
- 다시 손으로 써보면서 차근차근 순서를 생각해보자!
- N, M을 입력받고 출력을 위한 arr 배열과, 방문여부를 가릴 visit 배열을 전역변수로 선언
	- visit = [F F F F F] (편의상 0번 인덱스는 생각하지 않을 예정)
	- arr = [0 0]
- dfs(0) 호출
	- depth == M 이 아니므로 아래 for문으로 이동
	- i = 1, depth = 0
	- visit = [T F F F]
	- arr = [1 0]
	- dfs(1) 호출 === 처음 dfs(1)을 호출한 곳 ===
		- depth == M 이 아니므로 아래 for문으로 이동
		- i = 1은 True이므로 i = 2부터 시작, depth = 1
		- visit = [T T F F]
		- arr = [1 2]
		- dfs(2) 호출 (return 되면 돌아오는 곳)
			- depth == M 이므로 sb에 값 저장([1, 2]) 후 return
		- return 된 dfs(1)
			- return 되었으므로 dfs(2)를 호출했던 다음 줄 실행
			- i = 2였으므로 visit[2] = false, visit = [T F F F]
			- i = 3, depth = 1 (i = 2까지 진행했었으므로)
			- visit = [T F T F]
			- arr = [1 3]
			- dfs(2) 호출 (return 되면 돌아오는 곳)
				- depth == M 이므로 sb에 값 저장([1, 3]) 후 return
		- return 된 dfs(1)
			- return 되었으므로 dfs(2)를 호출했던 다음 줄 실행
			- i = 3 이었으므로 visit[3] = falst, visit = [T F F F]
			- i = 4, depth = 1
			- visit = [T F F T]
			- arr = [1 4]
			- dfs(2) 호출 (return 되면 돌아오는 곳)
				- depth == M 이므로 sb에 값 저장([1, 4]) 후 return
		- return 된 dfs(1)
			- return 되었으므로 dfs(2)를 호출했던 다음 줄 실행
			- i = 4 였으므로 visit[4] = false, visit = [T F F F]
			- for문이 종료되었으므로 dfs(1)을 처음 호출한 곳으로 돌아가기
	- 처음 dfs(1)을 호출한 곳으로 돌아와서 다음 줄 실행
	- i = 1이었으므로 visit[1] = false, visit = [F F F F]

	- i = 2, depth = 0 으로 i <= N 일 때까지 위 내용을 반복하게 된다!