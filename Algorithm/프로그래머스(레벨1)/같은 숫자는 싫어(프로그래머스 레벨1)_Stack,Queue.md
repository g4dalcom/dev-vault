# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12906)

## 📝 문제

배열 arr가 주어집니다. 배열 arr의 각 원소는 숫자 0부터 9까지로 이루어져 있습니다. 이때, 배열 arr에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 합니다. 단, 제거된 후 남은 수들을 반환할 때는 배열 arr의 원소들의 순서를 유지해야 합니다. 예를 들면,

-   arr = [1, 1, 3, 3, 0, 1, 1] 이면 [1, 3, 0, 1] 을 return 합니다.
-   arr = [4, 4, 4, 3, 3] 이면 [4, 3] 을 return 합니다.

배열 arr에서 연속적으로 나타나는 숫자는 제거하고 남은 수들을 return 하는 solution 함수를 완성해 주세요.

##### 제한사항

-   배열 arr의 크기 : 1,000,000 이하의 자연수
-   배열 arr의 원소의 크기 : 0보다 크거나 같고 9보다 작거나 같은 정수

---

##### 입출력 예

| arr             | answer    |
|:--------------- |:--------- |
| [1,1,3,3,0,1,1] | [1,3,0,1] |
| [4,4,4,3,3]     | [4, 3]          |


---

### 🔍 정답

```java
import java.util.*;

public class Solution {
    public int[] solution(int []arr) {
        Deque<Integer> dq = new ArrayDeque<>();
        dq.offer(arr[0]);
        for (int i = 1; i < arr.length; i++) {
            if (dq.getLast() == arr[i]) continue;
            dq.offer(arr[i]);
        }

        int size = dq.size();
        int[] answer = new int[size];
        for (int i = 0; i < size; i++) {
            answer[i] = dq.poll();
        }
        
        return answer;
    }
}
```
- List에 arr 요소들을 하나씩 넣되 연속적으로 같은 숫자가 나타는 것을 제외해야 하기 때문에 List 마지막 요소가 arr 요소와 같은지 비교를 했다.
- 그리고 List에 있는 것들을 answer 배열에 넘겨서 출력하였다.
- 요소를 넣을 때는 List의 마지막 요소를 확인해야 하고 출력할 때는 첫번째 요소부터 출력해야 하기 때문에 양쪽으로 입출력이 가능한 Deque를 사용했다!


### 🔎 다른 사람 풀이

```java
import java.util.*;

public class Solution {
    public int[] solution(int []arr) {
        ArrayList<Integer> tempList = new ArrayList<Integer>();
        int preNum = 10;
        for(int num : arr) {
            if(preNum != num)
                tempList.add(num);
            preNum = num;
        }       
        int[] answer = new int[tempList.size()];
        for(int i=0; i<answer.length; i++) {
            answer[i] = tempList.get(i).intValue();
        }
        return answer;
    }
}
```
- 이것은 좋아요를 가장 많이 받은 풀이인데 문제의 조건이 0~9까지의 숫자만 요소가 될 수 있기 때문에 preNum이라는 변수를 처음에 10으로 할당하고 이후에는 List에 들어가는 수로 갱신해서 중복을 피하는 방법이었다.