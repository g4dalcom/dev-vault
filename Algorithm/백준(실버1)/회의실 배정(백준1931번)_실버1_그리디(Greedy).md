# [문제링크](https://www.acmicpc.net/problem/1931)

## 📝 문제

한 개의 회의실이 있는데 이를 사용하고자 하는 N개의 회의에 대하여 회의실 사용표를 만들려고 한다. 각 회의 I에 대해 시작시간과 끝나는 시간이 주어져 있고, 각 회의가 겹치지 않게 하면서 회의실을 사용할 수 있는 회의의 최대 개수를 찾아보자. 단, 회의는 한번 시작하면 중간에 중단될 수 없으며 한 회의가 끝나는 것과 동시에 다음 회의가 시작될 수 있다. 회의의 시작시간과 끝나는 시간이 같을 수도 있다. 이 경우에는 시작하자마자 끝나는 것으로 생각하면 된다.

## 입력

첫째 줄에 회의의 수 N(1 ≤ N ≤ 100,000)이 주어진다. 둘째 줄부터 N+1 줄까지 각 회의의 정보가 주어지는데 이것은 공백을 사이에 두고 회의의 시작시간과 끝나는 시간이 주어진다. 시작 시간과 끝나는 시간은 231-1보다 작거나 같은 자연수 또는 0이다.

## 출력

첫째 줄에 최대 사용할 수 있는 회의의 최대 개수를 출력한다.

## 예제 입력 1

11
1 4
3 5
0 6
5 7
3 8
5 9
6 10
8 11
8 12
2 13
12 14

## 예제 출력 1 

4


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
  
        int[][] time = new int[N][2];  
  
        for (int i = 0; i < N; i++) {  
            StringTokenizer st = new StringTokenizer(br.readLine());  
            time[i][0] = Integer.parseInt(st.nextToken());      // 시작시간  
            time[i][1] = Integer.parseInt(st.nextToken());      // 종료시간  
        }  
  
        Arrays.sort(time, new Comparator<int[]>() {  
  
            // o1 = 시작시간, o2 = 종료시간  
            @Override  
            public int compare(final int[] o1, final int[] o2) {  
  
                // 종료시간을 기준으로 정렬을 하되, 종료시간이 같다면 시작시간을 기준으로 정렬!  
                if (o1[1] == o2[1]) return o1[0] - o2[0];  
  
                return o1[1] - o2[1];  
            }  
        });  
  
        int cnt = 0;                // 회의 개수 카운팅  
        int prev_end_time = 0;      // 이전 종료 시간  
  
        // 회의의 시작시간이 이전 종료시간과 같거나 보다 크다면 이전 종료 시간을 갱신하면서 카운팅을 한다!  
        for (int i = 0; i < N; i++) {  
            if (prev_end_time <= time[i][0]) {  
                prev_end_time = time[i][1];  
                cnt++;  
            }  
        }  
        System.out.println(cnt);  
    }  
}
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbwBJVp%2FbtrVoFUgIZ7%2F68lqj45WHuu3WNwZnqfXkk%2Fimg.png)
- 정렬 전 입력값

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDhEQN%2FbtrVpvDCLYV%2FGzRKeXeeTMED14ZmflFRP0%2Fimg.png)
- 종료시간으로 정렬한 입력값
- 종료시간순으로 정렬한 후에, 회의 시작시간이 이전 종료 시간과 같거나 보다 크다면 선택을 해준다.
- 또한,  다음과 같은 입력이 있다고 가정하면,

```
8 8
4 8
1 3
```

- 그리고 종료시점으로만 기준으로 정렬하게 된다면 다음과 같이 정렬될 수 있다.

```
1 3
8 8
4 8
```

- 위와같이 정렬되면 (1, 3) 이 선택되고 prev_end_time은 3으로 바뀔 것이다. 그 다음 (8, 8)이 선택되면 prev_end_time은 8로 바뀌어 버린다. 그렇게 되면 (4, 8)은 선택할 수 없게 되는 것이다. 즉, count 값은 2가 되어버리는 것이다.
- 하지만 이는 답이 아니다. 실제로는 (1, 3), (4, 8), (8, 8) 모두 선택되어 count 값은 3이어야 한다. 즉, 위와같은 불상사(?) 혹은 예외를 발생시키는 케이스들을 고려해서
- 만약 종료시점이 같다면 시작시점을 기준으로 오름차순으로 정렬해주어야 한다.