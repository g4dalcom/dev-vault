# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/138476)

## 📝 문제

경화는 과수원에서 귤을 수확했습니다. 경화는 수확한 귤 중 'k'개를 골라 상자 하나에 담아 판매하려고 합니다. 그런데 수확한 귤의 크기가 일정하지 않아 보기에 좋지 않다고 생각한 경화는 귤을 크기별로 분류했을 때 서로 다른 종류의 수를 최소화하고 싶습니다.

예를 들어, 경화가 수확한 귤 8개의 크기가 [1, 3, 2, 5, 4, 5, 2, 3] 이라고 합시다. 경화가 귤 6개를 판매하고 싶다면, 크기가 1, 4인 귤을 제외한 여섯 개의 귤을 상자에 담으면, 귤의 크기의 종류가 2, 3, 5로 총 3가지가 되며 이때가 서로 다른 종류가 최소일 때입니다.

경화가 한 상자에 담으려는 귤의 개수 `k`와 귤의 크기를 담은 배열 `tangerine`이 매개변수로 주어집니다. 경화가 귤 k개를 고를 때 크기가 서로 다른 종류의 수의 최솟값을 return 하도록 solution 함수를 작성해주세요.

---

##### 제한사항

- 1 ≤ `k` ≤ `tangerine`의 길이 ≤ 100,000
- 1 ≤ `tangerine`의 원소 ≤ 10,000,000

---

##### 입출력 예

|k|tangerine|result|
|---|---|---|
|6|[1, 3, 2, 5, 4, 5, 2, 3]|3|
|4|[1, 3, 2, 5, 4, 5, 2, 3]|2|
|2|[1, 1, 1, 1, 2, 2, 2, 3]|1|

---

##### 입출력 예 설명

**입출력 예 #1**

- 본문에서 설명한 예시입니다.

**입출력 예 #2**

- 경화는 크기가 2인 귤 2개와 3인 귤 2개 또는 2인 귤 2개와 5인 귤 2개 또는 3인 귤 2개와 5인 귤 2개로 귤을 판매할 수 있습니다. 이때의 크기 종류는 2가지로 이 값이 최소가 됩니다.

**입출력 예 #3**

- 경화는 크기가 1인 귤 2개를 판매하거나 2인 귤 2개를 판매할 수 있습니다. 이때의 크기 종류는 1가지로, 이 값이 최소가 됩니다.

---

### 💡 풀이

- 가장 쉽게 떠올릴 수 있는 방법은, 귤의 크기별 개수를 내림차순으로 정렬한 뒤 k가 0이 될 때까지 개수를 빼주면서 카운팅하는 것입니다.
- k = 4, tangerine = \[1, 3, 2, 5, 4, 5, 2, 3\] 일 때, 귤의 개수별 내림차순으로 정렬을 하면 tangerine = \[2, 2, 2, 1, 1\] 이 됩니다. 어떤 크기의 귤이 몇 개인지는 알 필요가 없으니 크기로만 정렬하면 됩니다.
- 그러면 두 번째 인덱스에서 k < 0 이 되므로 정답은 2가 되는 것이죠!
- 위 내용대로 구현을 하였는데, 우선 같은 크기의 귤을 묶어주어야 하므로 여기서는 HashMap을 사용하였습니다.

```java
HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < tangerine.length; i++) {
            map.put(tangerine[i], map.getOrDefault(tangerine[i], 0) + 1);
        }
```

- 위와 같이 해시맵에 크기와 개수를 담아주면 같은 크기는 1씩 카운팅이 되며 담기겠죠!

```java
ArrayList<Integer> keySet = new ArrayList<>(map.keySet());
        keySet.sort((o1, o2) -> map.get(o2).compareTo(map.get(o1)));
```

- 그런 후에 내림차순으로 정렬을 해줍니다.
- 이후에는 k < 0 이 될 때까지 빼기만 하면 되겠네요!

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(int k, int[] tangerine) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < tangerine.length; i++) {
            map.put(tangerine[i], map.getOrDefault(tangerine[i], 0) + 1);
        }
        
        ArrayList<Integer> keySet = new ArrayList<>(map.keySet());
        keySet.sort((o1, o2) -> map.get(o2).compareTo(map.get(o1)));
        
        int count = 0;
        int index = 0;
        while (k > 0) {
            count++;
            int curCount = map.get(keySet.get(index++));
            
            k -= curCount;
        }
        
        return count;
    }
}
```