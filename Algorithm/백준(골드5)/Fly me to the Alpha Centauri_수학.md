# [문제링크](https://www.acmicpc.net/problem/1011)

## 📝 문제

우현이는 어린 시절, 지구 외의 다른 행성에서도 인류들이 살아갈 수 있는 미래가 오리라 믿었다. 그리고 그가 지구라는 세상에 발을 내려 놓은 지 23년이 지난 지금, 세계 최연소 ASNA 우주 비행사가 되어 새로운 세계에 발을 내려 놓는 영광의 순간을 기다리고 있다.

그가 탑승하게 될 우주선은 Alpha Centauri라는 새로운 인류의 보금자리를 개척하기 위한 대규모 생활 유지 시스템을 탑재하고 있기 때문에, 그 크기와 질량이 엄청난 이유로 최신기술력을 총 동원하여 개발한 공간이동 장치를 탑재하였다. 하지만 이 공간이동 장치는 이동 거리를 급격하게 늘릴 경우 기계에 심각한 결함이 발생하는 단점이 있어서, 이전 작동시기에 k광년을 이동하였을 때는 k-1 , k 혹은 k+1 광년만을 다시 이동할 수 있다. 예를 들어, 이 장치를 처음 작동시킬 경우 -1 , 0 , 1 광년을 이론상 이동할 수 있으나 사실상 음수 혹은 0 거리만큼의 이동은 의미가 없으므로 1 광년을 이동할 수 있으며, 그 다음에는 0 , 1 , 2 광년을 이동할 수 있는 것이다. ( 여기서 다시 2광년을 이동한다면 다음 시기엔 1, 2, 3 광년을 이동할 수 있다. )

![](https://www.acmicpc.net/upload/201003/rlaehdgur.JPG)

김우현은 공간이동 장치 작동시의 에너지 소모가 크다는 점을 잘 알고 있기 때문에 x지점에서 y지점을 향해 최소한의 작동 횟수로 이동하려 한다. 하지만 y지점에 도착해서도 공간 이동장치의 안전성을 위하여 y지점에 도착하기 바로 직전의 이동거리는 반드시 1광년으로 하려 한다.

김우현을 위해 x지점부터 정확히 y지점으로 이동하는데 필요한 공간 이동 장치 작동 횟수의 최솟값을 구하는 프로그램을 작성하라.

## 입력

입력의 첫 줄에는 테스트케이스의 개수 T가 주어진다. 각각의 테스트 케이스에 대해 현재 위치 x 와 목표 위치 y 가 정수로 주어지며, x는 항상 y보다 작은 값을 갖는다. (0 ≤ x < y < 231)

## 출력

각 테스트 케이스에 대해 x지점으로부터 y지점까지 정확히 도달하는데 필요한 최소한의 공간이동 장치 작동 횟수를 출력한다.

## 예제 입력 1 

3
0 3
1 5
45 50

## 예제 출력 1 

3
3
4

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());

        while (T-- > 0) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int x = Integer.parseInt(st.nextToken());
            int y = Integer.parseInt(st.nextToken());

            int distance = y - x;
            int max = (int) Math.sqrt(distance);

            if (max == Math.sqrt(distance)) System.out.println(max * 2 - 1);
            else if (distance <= max * max + max) System.out.println(max * 2);
            else System.out.println(max * 2 + 1);
        }
    }
}
```

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fph0C1%2FbtrXwiaJFHt%2FrEdPoTqGsUEd8BOCb4iwi0%2Fimg.jpg)
- y - x 로 거리를 구해서 개수를 구할 수 있을 거라고 생각은 하고 접근을 하였고 경로를 일일이 써본 결과 규칙만 찾으면 해결할 수 있을 것 같았다.
- 개수가 변하는 시점과 경로에서 새로운 숫자가 나타나는 시점을 체크하고 규칙을 찾아보려고 했는데 쉽지가 않았다 ㅠㅠ
- [참고 사이트](https://st-lab.tistory.com/79)
- 결국 다른 풀이를 참고하였는데 규칙을 찾기 위해서는 자료를 다각도로 변화시켜서 바라볼 필요가 있다는 것을 깨달았다..


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FZsOsC%2FbtrXrNbfSI1%2FwKNwPcJoKqkfvuKiZqQWPk%2Fimg.png)
- 나와 같은 자료이지만 내가 거리를 기준으로 정렬을 하였다면 이 자료는 카운트(개수)를 기준으로 정렬하였다. 문제에서 구해야 하는 것은 결국 카운트이기 때문에 카운트를 기준으로 정렬했어야 했다 ㅠㅠ
- 어쨌든, 정확한 규칙을 찾기 위해 거리를 좀 더 넓게 보면 다음과 같은 규칙이 있다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fwd8z9%2FbtrXm7vO1IR%2FLDqkbenD8jXKcFQpnZfQoK%2Fimg.png)

1. max값이 시작하는 지점의 distance는 max의 제곱이다.
```java
int max = (int) Math.sqrt(distance);
```

2. max값 사이에는 count가 2개 존재한다.
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbHMx4Z%2FbtrXwi9E40a%2FeazUkbzvhfbfgZkeWqAGfk%2Fimg.png)

3. 최초 max값과 count의 관계는 `max * 2 - 1` 이다.
```java
// distance가 max의 제곱인 경우
if (distance == Math.pow(max, 2)) System.out.println(max * 2 - 1);

// max값 사이에 존재하는 count 중 하나
else if (distance <= max * max + max) System.out.println(max * 2);

// max값 사이에 존재하는 count 중 다른 하나
else System.out.println(max * 2 + 1);
```

- 세 번째 규칙은 정말 찾기 어려웠을 것 같다...
- 아무튼 max = 3 인 범위를 구한다고 하면,
	- distance = 9라면 max 의 제곱이므로 첫 번째 if문에서 구해진다. 이 때 카운트의 값은 max * 2 - 1이다.
	- distance = 10~12 사이라면, max * max + max 보다 작거나 같은 경우이므로 두 번째 if문에서 구해진다. 이 때 카운트의 값은 max * 2이다.
	- 그 외 경우는 세 번째 if문에서 구해지고 카운트는 max * 2 + 1이 된다.
- 코드 자체는 간결하고 직관적인데, 해설을 보면서도 여러번 위아래 훑어가며 이해했어야 했다 ㅠㅠ 혼자서 규칙을 찾으려면 수학 관련 문제도 여러 번 풀어봐야 될 것 같다는 생각이 들었다.
