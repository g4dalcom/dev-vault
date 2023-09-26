# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/131127)

## 📝 문제

XYZ 마트는 일정한 금액을 지불하면 10일 동안 회원 자격을 부여합니다. XYZ 마트에서는 회원을 대상으로 매일 한 가지 제품을 할인하는 행사를 합니다. 할인하는 제품은 하루에 하나씩만 구매할 수 있습니다. 알뜰한 정현이는 자신이 원하는 제품과 수량이 할인하는 날짜와 10일 연속으로 일치할 경우에 맞춰서 회원가입을 하려 합니다.

예를 들어, 정현이가 원하는 제품이 바나나 3개, 사과 2개, 쌀 2개, 돼지고기 2개, 냄비 1개이며, XYZ 마트에서 15일간 회원을 대상으로 할인하는 제품이 날짜 순서대로 치킨, 사과, 사과, 바나나, 쌀, 사과, 돼지고기, 바나나, 돼지고기, 쌀, 냄비, 바나나, 사과, 바나나인 경우에 대해 알아봅시다. 첫째 날부터 열흘 간에는 냄비가 할인하지 않기 때문에 첫째 날에는 회원가입을 하지 않습니다. 둘째 날부터 열흘 간에는 바나나를 원하는 만큼 할인구매할 수 없기 때문에 둘째 날에도 회원가입을 하지 않습니다. 셋째 날, 넷째 날, 다섯째 날부터 각각 열흘은 원하는 제품과 수량이 일치하기 때문에 셋 중 하루에 회원가입을 하려 합니다.

정현이가 원하는 제품을 나타내는 문자열 배열 `want`와 정현이가 원하는 제품의 수량을 나타내는 정수 배열 `number`, XYZ 마트에서 할인하는 제품을 나타내는 문자열 배열 `discount`가 주어졌을 때, 회원등록시 정현이가 원하는 제품을 모두 할인 받을 수 있는 회원등록 날짜의 총 일수를 return 하는 solution 함수를 완성하시오. 가능한 날이 없으면 0을 return 합니다.

---

##### 제한사항

- 1 ≤ `want`의 길이 = `number`의 길이 ≤ 10
    - 1 ≤ `number`의 원소 ≤ 10
    - `number[i]`는 `want[i]`의 수량을 의미하며, `number`의 원소의 합은 10입니다.
- 10 ≤ `discount`의 길이 ≤ 100,000
- `want`와 `discount`의 원소들은 알파벳 소문자로 이루어진 문자열입니다.
    - 1 ≤ `want`의 원소의 길이, `discount`의 원소의 길이 ≤ 12

---

##### 입출력 예

|want|number|discount|result|
|---|---|---|---|
|["banana", "apple", "rice", "pork", "pot"]|[3, 2, 2, 2, 1]|["chicken", "apple", "apple", "banana", "rice", "apple", "pork", "banana", "pork", "rice", "pot", "banana", "apple", "banana"]|3|
|["apple"]|[10]|["banana", "banana", "banana", "banana", "banana", "banana", "banana", "banana", "banana", "banana"]|0|

---

##### 입출력 예 설명

**입출력 예 #1**

- 문제 예시와 같습니다.

**입출력 예 #2**

- 사과가 할인하는 날이 없으므로 0을 return 합니다.

---

### 💡 풀이

- 기본적인 접근법은 해시맵을 이용해서 10일 간 할인하는 제품과 수량을 등록한 뒤 원하는 제품과 수량을 구매할 수 있는지를 확인하는 것입니다.
- 따라서 초기값으로 해시맵에 discount의 0~9번 인덱스 값을 넣어주었습니다.
- 그리고 10일 연속으로 할인하는 제품들과 비교해야 하므로 반복은 discount의 길이가 10보다 큰 동안만 하므로 `int roop = discount.length - 9;` 로 설정하였습니다.
- discount가 10이라면 한 번만 확인할 수 있겠죠!
- 그 후 반복문에서의 순서는 아래와 같습니다.
	- `isPossiblePurchase`, 현재 10일간 할인 제품들 목록으로 원하는 제품과 수량을 채울 수 있는지 확인
	- `removeProduct`, 다음 탐색을 위해 가장 첫 날 할인 제품을 제외
	- 마지막 다음 날의 할인 제품을 추가

### 🔍 정답

```java
import java.util.*;

class Solution {
    static HashMap<String, Integer> map = new HashMap<>();
    
    public int solution(String[] want, int[] number, String[] discount) {
        int possibleCount = 0;
        int removeIndex = 0;
        int addIndex = 10;
        
        for (int i = 0; i < 10; i++) {
            map.put(discount[i], map.getOrDefault(discount[i], 0) + 1);
        }
        
        int roop = discount.length - 9;  
        while (roop-- > 0) { 
            if (isPossiblePurchase(want, number)) {
                possibleCount++;
            }
            
            String removeTarget = discount[removeIndex++];
            if (map.containsKey(removeTarget)) {
                removeProduct(removeTarget);
            }

            if (discount.length > addIndex) {
                map.put(discount[addIndex], map.getOrDefault(discount[addIndex], 0) + 1);
                addIndex++;
            }
        }
        
        return possibleCount;
    }
    
    public boolean isPossiblePurchase(String[] want, int[] number) {
        for (int i = 0; i < want.length; i++) {
            String product = want[i];
            if (!map.containsKey(product) || map.get(product) < number[i]) {
                return false;
            }
        }
        return true;
    }
    
    public void removeProduct(String removeTarget) {
        if (map.get(removeTarget) > 1) {
            map.replace(removeTarget, map.get(removeTarget) - 1);
        } else {
            map.remove(removeTarget);
        }
    }
}
```