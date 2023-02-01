# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42862)

## 📝 문제

점심시간에 도둑이 들어, 일부 학생이 체육복을 도난당했습니다. 다행히 여벌 체육복이 있는 학생이 이들에게 체육복을 빌려주려 합니다. 학생들의 번호는 체격 순으로 매겨져 있어, 바로 앞번호의 학생이나 바로 뒷번호의 학생에게만 체육복을 빌려줄 수 있습니다. 예를 들어, 4번 학생은 3번 학생이나 5번 학생에게만 체육복을 빌려줄 수 있습니다. 체육복이 없으면 수업을 들을 수 없기 때문에 체육복을 적절히 빌려 최대한 많은 학생이 체육수업을 들어야 합니다.

전체 학생의 수 n, 체육복을 도난당한 학생들의 번호가 담긴 배열 lost, 여벌의 체육복을 가져온 학생들의 번호가 담긴 배열 reserve가 매개변수로 주어질 때, 체육수업을 들을 수 있는 학생의 최댓값을 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

-   전체 학생의 수는 2명 이상 30명 이하입니다.
-   체육복을 도난당한 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
-   여벌의 체육복을 가져온 학생의 수는 1명 이상 n명 이하이고 중복되는 번호는 없습니다.
-   여벌 체육복이 있는 학생만 다른 학생에게 체육복을 빌려줄 수 있습니다.
-   여벌 체육복을 가져온 학생이 체육복을 도난당했을 수 있습니다. 이때 이 학생은 체육복을 하나만 도난당했다고 가정하며, 남은 체육복이 하나이기에 다른 학생에게는 체육복을 빌려줄 수 없습니다.

##### 입출력 예

| n   | lost   | reserve   | return |
|:--- |:------ |:--------- |:------ |
| 5   | [2, 4] | [1, 3, 5] | 5      |
| 5   | [2, 4] | [3]       | 4      |
| 3   | [3]    | [1]       | 2       |


##### 입출력 예 설명

예제 #1  
1번 학생이 2번 학생에게 체육복을 빌려주고, 3번 학생이나 5번 학생이 4번 학생에게 체육복을 빌려주면 학생 5명이 체육수업을 들을 수 있습니다.

예제 #2  
3번 학생이 2번 학생이나 4번 학생에게 체육복을 빌려주면 학생 4명이 체육수업을 들을 수 있습니다.

---

### 🔍 정답

```java
import java.util.*;
import java.util.stream.Collectors;

class Solution {
    public int solution(int n, int[] lost, int[] reserve) {

		// 각 배열 내 중복 요소 제거
        Set<Integer> setLost = Arrays.stream(lost).boxed().collect(Collectors.toSet());
        Set<Integer> setReserve = Arrays.stream(reserve).boxed().collect(Collectors.toSet());

		// setLost와 setReserve의 중복요소 담기
        Set<Integer> retain = new HashSet(setReserve);
        retain.retainAll(setLost);

		// 두 배열의 중복 요소 제거
        setLost.removeAll(retain);
        setReserve.removeAll(retain);
        
        int answer = n - setLost.size();
        
        for (int r : setLost) {
            if (setReserve.contains(r-1)) {
                answer++;
                setReserve.remove(r-1);
            } else if (setReserve.contains(r+1)) {
                answer++;
                setReserve.remove(r+1);
            }
        }
    
        return answer;
    }
}
```
- 생각보다 엄청 까다로웠다 ㅠㅠ 
- 예제 입출력 케이스대로만 풀었더니 테스트 케이스의 거의 절반이 실패하길래 찾아보니 주어진 배열 내 중복 요소가 있을 수도 있고 정렬이 되어있지 않은 경우도 있고, 여분의 체육복이 있지만 체육복을 분실한 학생이 있는 케이스도 있었다.
- 입력값이 어떻게 들어올지에 대해 여러 방면으로 고려해봐야 한다는 걸 느꼈다...
- 어쨌든 Set을 이용해서 배열 내 중복요소를 제거하면서 정렬까지 하도록 하였고 두 배열의 중복 요소(여분의 체육복이 있지만 체육복을 분실해서 빌려줄 수 없는 경우)도 제거한 뒤에 값을 구했다.

```java
class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        int[] people = new int[n];
        int answer = n;

        for (int l : lost) 
            people[l-1]--;
            
        for (int r : reserve) 
            people[r-1]++;

        for (int i = 0; i < people.length; i++) {
            if(people[i] == -1) {
                if(i-1 >= 0 && people[i-1] == 1) {
                    people[i]++;
                    people[i-1]--;
                    
                }else if(i+1 < people.length && people[i+1] == 1) {
                    people[i]++;
                    people[i+1]--;
                    
                }else 
                    answer--;
            }
        }
        return answer;
    }
}
```
- 가장 좋아요를 많이 받은 해답인데 풀이가 정말 신박해서 감탄했다!
- 학생 수만큼의 배열을 만들어놓고 체육복을 잃어버린 학생은 -1, 여분이 있지만 읽어버린 학생은 0, 여분이 있어서 빌려줄 수 있는 학생은 1로 설정을 하고
- 체육복을 빌린 학생은 ++ 해서 0, 빌려준 학생은 -- 해서 0으로 설정하고 조건에 맞는 경우가 없다면 체육 수업을 들을 수 없는 학생이므로 한 명씩 - 해서 답을 구했다.
