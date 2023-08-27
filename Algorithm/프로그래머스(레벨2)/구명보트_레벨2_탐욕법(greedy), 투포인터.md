# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42885)

## 📝 문제

무인도에 갇힌 사람들을 구명보트를 이용하여 구출하려고 합니다. 구명보트는 작아서 한 번에 최대 **2명**씩 밖에 탈 수 없고, 무게 제한도 있습니다.

예를 들어, 사람들의 몸무게가 [70kg, 50kg, 80kg, 50kg]이고 구명보트의 무게 제한이 100kg이라면 2번째 사람과 4번째 사람은 같이 탈 수 있지만 1번째 사람과 3번째 사람의 무게의 합은 150kg이므로 구명보트의 무게 제한을 초과하여 같이 탈 수 없습니다.

구명보트를 최대한 적게 사용하여 모든 사람을 구출하려고 합니다.

사람들의 몸무게를 담은 배열 people과 구명보트의 무게 제한 limit가 매개변수로 주어질 때, 모든 사람을 구출하기 위해 필요한 구명보트 개수의 최솟값을 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 무인도에 갇힌 사람은 1명 이상 50,000명 이하입니다.
- 각 사람의 몸무게는 40kg 이상 240kg 이하입니다.
- 구명보트의 무게 제한은 40kg 이상 240kg 이하입니다.
- 구명보트의 무게 제한은 항상 사람들의 몸무게 중 최댓값보다 크게 주어지므로 사람들을 구출할 수 없는 경우는 없습니다.

##### 입출력 예

|people|limit|return|
|---|---|---|
|[70, 50, 80, 50]|100|3|
|[70, 80, 50]|100|3|

---

### 💡 풀이

- 보트를 가장 적게 사용하려면 최대한 limit에 근접하게 사용해야 합니다. 그런데 여기서 또 하나의 조건은 보트에 두 명 밖에 타지 못한다는 것입니다.
- 이것은 문제를 쉽게 만드는 조건이었다고 생각합니다.
- 최대한 몸무게가 무거운 사람을 두 명이 타는 보트에 속하도록 구하면 되겠다고 생각했고 이것을 투포인터로 구현해보았습니다.
- 기본적인 방법은 각 인덱스의 끝 점에 포인터를 두고 limit가 허용하는 범위까지 right를 - 하는 것입니다.
- 그러면 자연히 가장 몸무게가 무거운 사람이 가벼운 사람과 맞춰져서 limit에 근접하게 되겠죠!
- 그런데 여기서 만약에 people[left]가 이미 limit를 넘는 상황이라면 우리는 그 이후의 계산은 할 필요가 없습니다. 무조건 보트에 한 명씩 타는 경우 밖에 없으니까요!

```java
if (people[left] >= limit) {
	answer += right - left;
	break;
}
```

- 따라서 위와 같이 남은 인원은 모두 보트에 혼자 타는 경우로 계산하고 answer에 추가해주었습니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(int[] people, int limit) {
        int answer = 0;
        int left = 0;
        int right = people.length-1;
        
        Arrays.sort(people);
        
        while (left <= right) {
            if (people[left] >= limit) {
                answer += right - left;
                break;
            }
            
            if (people[left] + people[right] > limit) {
                right--;
            } else {
                left++;
                right--;
            }
            answer++;
        }
        
        return answer;
    }
}
```