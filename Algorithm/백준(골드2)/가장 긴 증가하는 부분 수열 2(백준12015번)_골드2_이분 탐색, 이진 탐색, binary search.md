# [문제링크](https://www.acmicpc.net/problem/12015)

## 📝 문제

수열 A가 주어졌을 때, 가장 긴 증가하는 부분 수열을 구하는 프로그램을 작성하시오.

예를 들어, 수열 A = {10, 20, 10, 30, 20, 50} 인 경우에 가장 긴 증가하는 부분 수열은 A = {**10**, **20**, 10, **30**, 20, **50**} 이고, 길이는 4이다.

## 입력

첫째 줄에 수열 A의 크기 N (1 ≤ N ≤ 1,000,000)이 주어진다.

둘째 줄에는 수열 A를 이루고 있는 Ai가 주어진다. (1 ≤ Ai ≤ 1,000,000)

## 출력

첫째 줄에 수열 A의 가장 긴 증가하는 부분 수열의 길이를 출력한다.

## 예제 입력 1 

6
10 20 10 30 20 50

## 예제 출력 1 

4

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());

        StringTokenizer st = new StringTokenizer(br.readLine());

        int[] input = new int[N];
        ArrayList<Integer> seq = new ArrayList<>();     // 수열 담을 리스트

        for (int i = 0; i < N; i++) {
            input[i] = Integer.parseInt(st.nextToken());
        }

        for (int i = 0; i < N; i++) {
            // 첫 요소 혹은 리스트의 마지막 요소보다 현재 탐색하는 input값이 더 크다면 리스트에 넣는다.
            if (seq.size() == 0 || seq.get(seq.size()-1) < input[i]) {
                seq.add(input[i]);
            }
            
            /**
             * 현재 탐색값이 마지막 요소보다 작은 경우, 리스트에 담긴 수열 중 교체가 가능한 부분이 있는 확인해야 하는데
             * 그것을 이분 탐색으로 진행한다.
             * seq = [10, 20, 30] 일 때 input값이 15라면 10, 20 을 확인해서 input값 15보다 큰 수가 처음 나오는 인덱스를 구한 후(hi)
             * 그 인덱스에 input값을 넣어서 대체한다. (seq.set(lo, input))
             */
            else {
                int lo = 0;
                int hi = seq.size()-1;

                while (lo < hi) {
                    int mid = (lo + hi) / 2;

                    if (seq.get(mid) < input[i]) {
                        lo = mid + 1;
                    } else {
                        hi = mid;
                    }
                }

                seq.set(lo, input[i]);
            }
        }
        System.out.println(seq.size());
    }
}
```
- 가장 긴 증가하는 부분 수열 1처럼 dp로 풀면 시간 초과가 나는 문제이다.
- 풀이법을 생각하지 못해서 참고 사이트를 보고 이해하였는데, 증가하는 수열을 출력하는 것이 아니라 길이만 출력하면 되는 것이라서 가능한 풀이법이다.
- 증가하는 수열을 그대로 출력을 할 땐 잘못된 값이 출력될 수 있다!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FRYH0H%2FbtseBcsownH%2F7gWqNtJnyDfnSn3FY3Iom0%2Fimg.png)

- 기본 풀이법은 증가하는 수열을 리스트에 순서대로 입력하되, 
- 증가하는 수열이 아닌 경우에는 현재값을 입력된 리스트들 중 증가하는 수열이 될 수 있는 자리에 대체하도록 하는 것이다. 이 때 사용하는 것이 이분 탐색이다.
- 이분 탐색으로 구하고자 하는 값이 target(현재 input)을 초과하는 가장 가까운 수이므로 lower bound와 upper bound 둘 다 가능하다.
- [참고 사이트 1](https://st-lab.tistory.com/285)
- [참고 사이트 2](https://velog.io/@jodawooooon/Java-BOJ-12015-%EA%B0%80%EC%9E%A5-%EA%B8%B4-%EC%A6%9D%EA%B0%80%ED%95%98%EB%8A%94-%EB%B6%80%EB%B6%84-%EC%88%98%EC%97%B4-2-%EC%9D%B4%EB%B6%84%ED%83%90%EC%83%89)