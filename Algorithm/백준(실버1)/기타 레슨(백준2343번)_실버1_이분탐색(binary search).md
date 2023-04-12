# [문제링크](https://www.acmicpc.net/problem/2343)

## 📝 문제

강토는 자신의 기타 강의 동영상을 블루레이로 만들어 판매하려고 한다. 블루레이에는 총 N개의 강의가 들어가는데, 블루레이를 녹화할 때, 강의의 순서가 바뀌면 안 된다. 순서가 뒤바뀌는 경우에는 강의의 흐름이 끊겨, 학생들이 대혼란에 빠질 수 있기 때문이다. 즉, i번 강의와 j번 강의를 같은 블루레이에 녹화하려면 i와 j 사이의 모든 강의도 같은 블루레이에 녹화해야 한다.

강토는 이 블루레이가 얼마나 팔릴지 아직 알 수 없기 때문에, 블루레이의 개수를 가급적 줄이려고 한다. 오랜 고민 끝에 강토는 M개의 블루레이에 모든 기타 강의 동영상을 녹화하기로 했다. 이때, 블루레이의 크기(녹화 가능한 길이)를 최소로 하려고 한다. 단, M개의 블루레이는 모두 같은 크기이어야 한다.

강토의 각 강의의 길이가 분 단위(자연수)로 주어진다. 이때, 가능한 블루레이의 크기 중 최소를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 강의의 수 N (1 ≤ N ≤ 100,000)과 M (1 ≤ M ≤ N)이 주어진다. 다음 줄에는 강토의 기타 강의의 길이가 강의 순서대로 분 단위로(자연수)로 주어진다. 각 강의의 길이는 10,000분을 넘지 않는다.

## 출력

첫째 줄에 가능한 블루레이 크기중 최소를 출력한다.

## 예제 입력 1 

9 3
1 2 3 4 5 6 7 8 9

## 예제 출력 1
17

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
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());   // 강의 수
        int M = Integer.parseInt(st.nextToken());   // 블루레이 수

        int[] lecture = new int[N];

        st = new StringTokenizer(br.readLine());

        // 초깃값 설정
        int left = Integer.MIN_VALUE;
        int right = 0;
        for (int i = 0; i < N; i++) {
            lecture[i] = Integer.parseInt(st.nextToken());

            left = Math.max(left, lecture[i]);
            right += lecture[i];
        }
        
        // 이분 탐색
        while (left <= right) {
            int mid = (left + right) / 2;
            
            int sum = 0;
            int cnt = 1;
            for (int i = 0; i < N; i++) {
                sum += lecture[i];
                if (sum > mid) {
                    sum = lecture[i];
                    cnt++;
                }
            }

            if (cnt > M) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        System.out.println(left);
    }
}
```
- 블루레이의 최솟값은 요소들 중 최댓값이 된다.
- 예를 들어 lecture = [7, 8, 9]이고 블루레이가 3개라면 각각 하나씩 담을 수 있는데 이 때 모든 블루레이의 크기는 같아야하므로 요소들의 최댓값인 9가 블루레이의 최솟값이 되는 것!
- 그리고 블루레이가 1개라면 lecture의 모든 요소를 하나에 담아야 하므로 블루레이의 값은 모든 요소의 합이 된다.
- 따라서, 최솟값 left는 요소들 중 최댓값으로 하고 최댓값 right는 모든 요소의 합으로 정하였다!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FxzAfT%2Fbtr9C3OLlOC%2FB96Cn2ZUhkuNZoHqiAFEmK%2Fimg.png)
- 이분 탐색의 대략적인 흐름은 그림과 같다!
- 중간값인 mid를 기준으로 mid보다 작거나 같을 때까지 요소를 더해서 묶어주고 덩어리의 개수(count)를 세어본다.
- 예시처럼 덩어리가 블루레이의 수보다 작다면 덩어리당 요소를 더 적게 포함시켜서 덩어리를 늘려야 하므로 mid가 작아져야 한다.

```java
if (cnt > M) {
	left = mid + 1;
} else {
	right = mid - 1;
}
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKGkNU%2Fbtr9Pe2f57O%2FCoGB5V3h8ySBbUppkTnOF1%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F37T1x%2Fbtr9N1IWiX7%2FfCFrHULdlklRLj7nKZtG30%2Fimg.png)
- 이러한 흐름으로 left가 right를 초과하는 순간까지 점점 탐색의 범위를 좁혀가며 최적값을 찾게된다.
- 일반적인 이분 탐색이라면 `cnt == M` 인 순간(target값과 일치해지는 순간) mid값을 리턴하면 되지만, 우리가 구해야하는 것은 M과 일치한 것뿐만 아니라 최솟값이어야 한다.
- 경우에 따라 cnt = 3 인 조합이 여러개 나올 수 있기 때문!
- 그러므로 left > right 가 되는 지점까지 탐색을 한 후에 left 값을 리턴하면 된다!