# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/67258)

## 📝 문제

개발자 출신으로 세계 최고의 갑부가 된 `어피치`는 스트레스를 받을 때면 이를 풀기 위해 오프라인 매장에 쇼핑을 하러 가곤 합니다.  
어피치는 쇼핑을 할 때면 매장 진열대의 특정 범위의 물건들을 모두 싹쓸이 구매하는 습관이 있습니다.  
어느 날 스트레스를 풀기 위해 보석 매장에 쇼핑을 하러 간 어피치는 이전처럼 진열대의 특정 범위의 보석을 모두 구매하되 특별히 아래 목적을 달성하고 싶었습니다.  
`진열된 모든 종류의 보석을 적어도 1개 이상 포함하는 가장 짧은 구간을 찾아서 구매`

예를 들어 아래 진열대는 4종류의 보석(RUBY, DIA, EMERALD, SAPPHIRE) 8개가 진열된 예시입니다.

|진열대 번호|1|2|3|4|5|6|7|8|
|---|---|---|---|---|---|---|---|---|
|보석 이름|DIA|RUBY|**RUBY**|**DIA**|**DIA**|**EMERALD**|**SAPPHIRE**|DIA|

진열대의 3번부터 7번까지 5개의 보석을 구매하면 모든 종류의 보석을 적어도 하나 이상씩 포함하게 됩니다.

진열대의 3, 4, 6, 7번의 보석만 구매하는 것은 중간에 특정 구간(5번)이 빠지게 되므로 어피치의 쇼핑 습관에 맞지 않습니다.

진열대 번호 순서대로 보석들의 이름이 저장된 배열 gems가 매개변수로 주어집니다. 이때 모든 보석을 하나 이상 포함하는 가장 짧은 구간을 찾아서 return 하도록 solution 함수를 완성해주세요.  
가장 짧은 구간의 `시작 진열대 번호`와 `끝 진열대 번호`를 차례대로 배열에 담아서 return 하도록 하며, 만약 가장 짧은 구간이 여러 개라면 `시작 진열대 번호`가 가장 작은 구간을 return 합니다.

##### **[제한사항]**

- gems 배열의 크기는 1 이상 100,000 이하입니다.
    - gems 배열의 각 원소는 진열대에 나열된 보석을 나타냅니다.
    - gems 배열에는 1번 진열대부터 진열대 번호 순서대로 보석이름이 차례대로 저장되어 있습니다.
    - gems 배열의 각 원소는 길이가 1 이상 10 이하인 알파벳 대문자로만 구성된 문자열입니다.

---

##### **입출력 예**

|gems|result|
|---|---|
|`["DIA", "RUBY", "RUBY", "DIA", "DIA", "EMERALD", "SAPPHIRE", "DIA"]`|[3, 7]|
|`["AA", "AB", "AC", "AA", "AC"]`|[1, 3]|
|`["XYZ", "XYZ", "XYZ"]`|[1, 1]|
|`["ZZZ", "YYY", "NNNN", "YYY", "BBB"]`|[1, 5]|

##### **입출력 예에 대한 설명**

**입출력 예 #1**  
문제 예시와 같습니다.

**입출력 예 #2**  
3종류의 보석(AA, AB, AC)을 모두 포함하는 가장 짧은 구간은 [1, 3], [2, 4]가 있습니다.  
`시작 진열대 번호`가 더 작은 [1, 3]을 return 해주어야 합니다.

**입출력 예 #3**  
1종류의 보석(XYZ)을 포함하는 가장 짧은 구간은 [1, 1], [2, 2], [3, 3]이 있습니다.  
`시작 진열대 번호`가 가장 작은 [1, 1]을 return 해주어야 합니다.

**입출력 예 #4**  
4종류의 보석(ZZZ, YYY, NNNN, BBB)을 모두 포함하는 구간은 [1, 5]가 유일합니다.  
그러므로 [1, 5]를 return 해주어야 합니다.

---

### 💡 풀이

- 가장 먼저 알아야 할 것은 보석의 종류가 몇 개인지입니다. 그래야 탐색을 할 때 모든 보석을 포함했는지 여부를 알 수 있기 때문입니다. 이것을 위해서 HashSet의 size() 메소드를 이용했습니다.

```java
int gemCount = new HashSet<>(Arrays.asList(gems)).size();
```

- 그 다음은 모든 보석을 포함하는 컬렉션을 만들어야 합니다. 예제 1번이 고민하기 굉장히 좋았었는데요.
- \["DIA", "RUBY", "RUBY", "DIA", "DIA", "EMERALD", "SAPPHIRE", "DIA"\] 가 주어졌을 때 1번 진열대부터 7번 진열대까지 갔을 때 모든 보석을 포함하게 되지만 가장 짧은 길이는 아니죠?! 이것을 더 짧은 길이로 구하는 것이 핵심 로직이었습니다.
- 우선 가장 먼저 중복되는 부분인 RUBY를 보면, 3번 진열장에서 RUBY가 중복되지만 우리는 RUBY의 중복을 제외해서는 안 됩니다. 왜냐하면 RUBY를 제외하면 앞선 1번 진열장의 DIA까지 제외를 해야 하는데 DIA가 또 언제 나올지 알 수 없기 때문입니다.
- 여기서 알 수 있는 것은 제외할 수 있는 보석은 가장 앞에 있는 보석이라는 점입니다. 그리고 가장 앞에 있는 보석을 제외할 수 있는 조건은 그 보석과 같은 보석이 등장했을 때겠죠!

```java
while (map.get(gems[start]) > 1) {
	map.replace(gems[start], map.get(gems[start]) - 1);
	start++;
}
```

- 따라서 보석의 종류를 key로, 개수를 value로 하는 해시맵에 보석과 개수를 넣어주면서 가장 앞선 보석과 중복된다면 제거하는 방식으로 모든 보석을 포함하는 컬렉션을 만들 수 있습니다.
- 그리고 진열장은 1번부터 시작하므로 결과값에 각각 +1 해주는 것도 잊지 말아야겠죠!

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int[] solution(String[] gems) {
        HashMap<String, Integer> map = new HashMap<>();
        int gemCount = new HashSet<>(Arrays.asList(gems)).size();
        int[] result = new int[2];
        int start = 0;
        int min = Integer.MAX_VALUE;
        
        for (int i = 0; i < gems.length; i++) {
            map.put(gems[i], map.getOrDefault(gems[i], 0) + 1);
            
            while (map.get(gems[start]) > 1) {
                map.replace(gems[start], map.get(gems[start]) - 1);
                start++;
            }

            if (map.size() == gemCount && min > i - start) {
                min = i - start;
                result[0] = start + 1;
                result[1] = i + 1;
            }
        }
        
        return result;
    }
}
```