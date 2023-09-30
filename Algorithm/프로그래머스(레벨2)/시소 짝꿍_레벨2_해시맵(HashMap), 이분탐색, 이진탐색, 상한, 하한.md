# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/152996)

## 📝 문제

어느 공원 놀이터에는 시소가 하나 설치되어 있습니다. 이 시소는 중심으로부터 2(m), 3(m), 4(m) 거리의 지점에 좌석이 하나씩 있습니다.  
이 시소를 두 명이 마주 보고 탄다고 할 때, 시소가 평형인 상태에서 각각에 의해 시소에 걸리는 토크의 크기가 서로 상쇄되어 완전한 균형을 이룰 수 있다면 그 두 사람을 시소 짝꿍이라고 합니다. 즉, 탑승한 사람의 무게와 시소 축과 좌석 간의 거리의 곱이 양쪽 다 같다면 시소 짝꿍이라고 할 수 있습니다.  
사람들의 몸무게 목록 `weights`이 주어질 때, 시소 짝꿍이 몇 쌍 존재하는지 구하여 return 하도록 solution 함수를 완성해주세요.

---

##### 제한 사항

- 2 ≤ `weights`의 길이 ≤ 100,000
- 100 ≤ `weights`[i] ≤ 1,000
    - 몸무게 단위는 N(뉴턴)으로 주어집니다.
    - 몸무게는 모두 정수입니다.

---

##### 입출력 예

|weights|result|
|---|---|
|[100,180,360,100,270]|4|

---

##### 입출력 예 설명

{100, 100} 은 서로 같은 거리에 마주보고 앉으면 균형을 이룹니다.  
{180, 360} 은 각각 4(m), 2(m) 거리에 마주보고 앉으면 균형을 이룹니다.  
{180, 270} 은 각각 3(m), 2(m) 거리에 마주보고 앉으면 균형을 이룹니다.  
{270, 360} 은 각각 4(m), 3(m) 거리에 마주보고 앉으면 균형을 이룹니다.

---

### 💡 풀이

- weights의 길이가 최대 100,000이기 때문에 2중 for문을 사용하면 약 100억 번의 연산을 해서 시간 초과가 발생합니다.
- 구현 방법을 엄청 고민했던 문제인데 ㅠㅠ 저는 해시맵을 이용하였습니다.
- 모든 수들을 해시맵에 저장하고 중복된 수들은 getOrDefault 메소드로 관리하였습니다. 그리고 현재 수를 가지고 짝꿍이 될 수 있는 수를 계산하여 그 값이 해시맵에 존재하는지를 확인하는 방법이었습니다.
- 현재 수가 180이라면 180의 2.0배, 1.5배, 4/3배가 존재하는지만 확인하면 되고 그 반대의 경우는 생각하지 않아도 됩니다. 어차피 해시맵에 모든 수를 처음에 넣어두었기 때문에 반대의 경우가 존재한다면 탐색 중에 어차피 계산이 되기 때문입니다.
- 그리고 1.5배와 4/3배를 계산하려면 소수점 계산이 필연적이라고 생각해서 double 타입으로 계산하였습니다.
- 시행착오를 겪었던 점은, 만약 100이라는 몸무게가 여러 명일 때 계산하는 것이었는데요.
- 저 같은 경우는 예제에서처럼 중복이 2개까지만 나올 것이라고 예상하고 map.get()을 했을 때 저장된 value가 2 이상이라면 같은 몸무게의 짝꿍이므로 +1만 카운팅을 해주었었는데 여러 명의 중복 인원이 나올 수가 있었습니다.
- 가령 100이 A, B, C 세 명이라면 AB, AC, BC 의 3가지 경우의 수가 생기고 네 명이라면 AB, AC, AD, BC, BD, CD 6가지의 경우의 수가 생깁니다. 이 모든 것을 계산해주어야 하는데 value를 v라고 했을 때,

$$v \times (v-1) / 2$$
- 위 수식으로 해당 값을 구할 수 있습니다.
- 그리고 만약 현재 value가 2이고 2.0배를 한 value가 3이라면 **a b / c d e** 이런 식으로 존재하는 것이므로 짝꿍의 수는 **현재 value * 계산한 value** 를 통해 구할 수 있습니다!

### 🔍 정답

```java
import java.util.*;

class Solution {
    public long solution(int[] weights) {
        long mateCount = 0;
        HashMap<Double, Integer> map = new HashMap<>();

        for (double weight : weights) {
            map.put(weight, map.getOrDefault(weight, 0) + 1);
        }

        for (double weight : map.keySet()) {
            double w = map.get(weight);

            if (map.containsKey(weight)) mateCount += w * (w-1) / 2;
            if (map.containsKey(weight * 2.0)) mateCount += w * map.get(weight * 2.0);
            if (map.containsKey(weight * 1.5)) mateCount += w * map.get(weight * 1.5);
            if (map.containsKey(weight / 3.0 * 4.0)) mateCount += w * map.get(weight / 3.0 * 4.0);
        }

        return mateCount;
    }
}
```


### 💡 다른 정답(이분 탐색)

```java
import java.util.Arrays;

class Solution {

    private static final int[][] rates = {{1, 1}, {3, 2}, {4, 2}, {4, 3}};

    public static long solution(int[] weights) {
        long answer = 0;

        // 정렬
        Arrays.sort(weights);

        // 비율 만큼 반복
        for (int[] rate : rates) {
            for (int i = 0; i < weights.length; i++) {
                int x = weights[i];

                // 비율 계산을 통해 y를 구함
                if (x * rate[0] % rate[1] != 0) continue;
                int y = ((x * rate[0]) / rate[1]);

                // y의 값이 i+1 부터 존재하는지 확인함
                int upper = upperBound(y, weights, i + 1, weights.length);
                
                // 하한 탐색은 상한 탐색의 위치 이하이므로 탐색 마지막 위치를 upper 해도 됨
                int lower = lowerBound(y, weights, i + 1, upper);

                // 상한과 하한의 값을 빼서 중복된 값이 몇 개 인지 정답에 더함
                // 만약 y의 값이 범위 내에 존재하지 않으면,
                // 상한과 하한 둘 다 i+1의 위치를 반환하기 때문에 둘의 차이는 0이 될 것임
                answer += (upper - lower);
            }
        }

        return answer;
    }

    // 상한
    private static int upperBound(int findWeight, int[] weights, int left, int right) {
        while (left < right) {
            int mid = (left + right) / 2;

            if (findWeight < weights[mid]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    // 하한
    private static int lowerBound(int findWeight, int[] weights, int left, int right) {
        while (left < right) {
            int mid = (left + right) / 2;

            if (findWeight <= weights[mid]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```

#### 비례식

- 2:3, 3:4 이러한 비율 계산을 위해서는 비례식을 세워야 합니다. 계산식은 아래와 같습니다.
$$x : y = a : b$$
- 내항의 곱은 외항의 곱과 같다는 점에 따라 아래와 같이 표현할 수 있고
$$ay = bx$$
- 이항을 통해 y를 구하는 식으로 바꿀 수 있습니다.
$$y = (bx) / a$$

#### 이진 탐색

- 이진 탐색은 기본적으로 정렬이 되어있어야 합니다.
- 그리고 비율 계산을 통해 구해진 **y의 값**이 **다음 위치**에 **존재**하는지 구하면 됩니다.
- **해당 비율의 값 y**를 가진 것이 있을 때는 짝이 존재 하는 것이고, 없으면 짝이 존재하지 않는 것입니다.

```java
int upper = upperBound(y, weights, i + 1, weights.length);
```

- 현재 위치가 **i** 입니다.
- **i+1** 부터 **배열 끝**까지 y의 값을 이진 탐색을 통해 존재하는지 확인합니다.
- 0 부터 i-1 번째 까지는 짝꿍 매칭을 이미 완료한 상태입니다.
- 이진 탐색의 **상한**과 **하한**을 구하는 이유는 배열에 **중복된 값**이 들어갈 수 있기 때문입니다.
- 상한과 하한의 **차이**를 구하면 중복된 값이 몇 개 있는지 확인할 수 있습니다.
- [참고 사이트](https://ksb-dev.tistory.com/266)