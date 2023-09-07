# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/49993)

## 📝 문제

선행 스킬이란 어떤 스킬을 배우기 전에 먼저 배워야 하는 스킬을 뜻합니다.

예를 들어 선행 스킬 순서가 `스파크 → 라이트닝 볼트 → 썬더`일때, 썬더를 배우려면 먼저 라이트닝 볼트를 배워야 하고, 라이트닝 볼트를 배우려면 먼저 스파크를 배워야 합니다.

위 순서에 없는 다른 스킬(힐링 등)은 순서에 상관없이 배울 수 있습니다. 따라서 `스파크 → 힐링 → 라이트닝 볼트 → 썬더`와 같은 스킬트리는 가능하지만, `썬더 → 스파크`나 `라이트닝 볼트 → 스파크 → 힐링 → 썬더`와 같은 스킬트리는 불가능합니다.

선행 스킬 순서 skill과 유저들이 만든 스킬트리[1](https://school.programmers.co.kr/learn/courses/30/lessons/49993#fn1)를 담은 배열 skill_trees가 매개변수로 주어질 때, 가능한 스킬트리 개수를 return 하는 solution 함수를 작성해주세요.

##### 제한 조건

- 스킬은 알파벳 대문자로 표기하며, 모든 문자열은 알파벳 대문자로만 이루어져 있습니다.
- 스킬 순서와 스킬트리는 문자열로 표기합니다.
    - 예를 들어, `C → B → D` 라면 "CBD"로 표기합니다
- 선행 스킬 순서 skill의 길이는 1 이상 26 이하이며, 스킬은 중복해 주어지지 않습니다.
- skill_trees는 길이 1 이상 20 이하인 배열입니다.
- skill_trees의 원소는 스킬을 나타내는 문자열입니다.
    - skill_trees의 원소는 길이가 2 이상 26 이하인 문자열이며, 스킬이 중복해 주어지지 않습니다.

##### 입출력 예

|skill|skill_trees|return|
|---|---|---|
|`"CBD"`|`["BACDE", "CBADF", "AECB", "BDA"]`|2|

##### 입출력 예 설명

- "BACDE": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트립니다.
- "CBADF": 가능한 스킬트리입니다.
- "AECB": 가능한 스킬트리입니다.
- "BDA": B 스킬을 배우기 전에 C 스킬을 먼저 배워야 합니다. 불가능한 스킬트리입니다.

---

### 💡 풀이

- 알파벳과 스킬 순서를 해시맵에 저장을 합니다.
- 순서가 상관없는 스킬은 건너뛰고 순서가 있는 스킬이 나왔을 경우 순서대로 나왔는지를 검증하도록 구현하였습니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(String skill, String[] skill_trees) {
        HashMap<Character, Integer> map = new HashMap<>();
        int possibleCount = 0;      // 가능한 스킬트리의 개수
        int order = 0;              // 해시맵에 저장할 스킬트리의 순서
        
        // 먼저 배워야 하는 스킬을 0부터 1씩 늘리며 차례로 해시맵에 저장
        char[] skills = skill.toCharArray();
        for (char s : skills) {
            map.put(s, order++);
        }
        
        for (int i = 0; i < skill_trees.length; i++) {
            int index = 0;
            char[] tree = skill_trees[i].toCharArray();
            boolean possible = true;
            
            /**
             * 순서가 상관없는 스킬은 건너뛰고
             * 순서가 필요한 스킬이 순서대로 나왔는지 확인
             */
            for (char s : tree) {
                if (!map.containsKey(s)) continue;
              
                if (map.get(s) > index) {
                    possible = false;
                    break;
                }
                index++;
            }    
            if (possible) possibleCount++;
        }
        
        return possibleCount;
    }
}
```
