# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42578)

## 📝 문제

코니는 매일 다른 옷을 조합하여 입는것을 좋아합니다.

예를 들어 코니가 가진 옷이 아래와 같고, 오늘 코니가 동그란 안경, 긴 코트, 파란색 티셔츠를 입었다면 다음날은 청바지를 추가로 입거나 동그란 안경 대신 검정 선글라스를 착용하거나 해야합니다.

|종류|이름|
|---|---|
|얼굴|동그란 안경, 검정 선글라스|
|상의|파란색 티셔츠|
|하의|청바지|
|겉옷|긴 코트|

- 코니는 각 종류별로 최대 1가지 의상만 착용할 수 있습니다. 예를 들어 위 예시의 경우 동그란 안경과 검정 선글라스를 동시에 착용할 수는 없습니다.
- 착용한 의상의 일부가 겹치더라도, 다른 의상이 겹치지 않거나, 혹은 의상을 추가로 더 착용한 경우에는 서로 다른 방법으로 옷을 착용한 것으로 계산합니다.
- 코니는 하루에 최소 한 개의 의상은 입습니다.

코니가 가진 의상들이 담긴 2차원 배열 clothes가 주어질 때 서로 다른 옷의 조합의 수를 return 하도록 solution 함수를 작성해주세요.

---

##### 제한사항

- clothes의 각 행은 [의상의 이름, 의상의 종류]로 이루어져 있습니다.
- 코니가 가진 의상의 수는 1개 이상 30개 이하입니다.
- 같은 이름을 가진 의상은 존재하지 않습니다.
- clothes의 모든 원소는 문자열로 이루어져 있습니다.
- 모든 문자열의 길이는 1 이상 20 이하인 자연수이고 알파벳 소문자 또는 '_' 로만 이루어져 있습니다.

##### 입출력 예

|clothes|return|
|---|---|
|[["yellow_hat", "headgear"], ["blue_sunglasses", "eyewear"], ["green_turban", "headgear"]]|5|
|[["crow_mask", "face"], ["blue_sunglasses", "face"], ["smoky_makeup", "face"]]|3|

##### 입출력 예 설명

예제 #1  
headgear에 해당하는 의상이 yellow_hat, green_turban이고 eyewear에 해당하는 의상이 blue_sunglasses이므로 아래와 같이 5개의 조합이 가능합니다.

```
1. yellow_hat
2. blue_sunglasses
3. green_turban
4. yellow_hat + blue_sunglasses
5. green_turban + blue_sunglasses
```

예제 #2  
face에 해당하는 의상이 crow_mask, blue_sunglasses, smoky_makeup이므로 아래와 같이 3개의 조합이 가능합니다.

```
1. crow_mask
2. blue_sunglasses
3. smoky_makeup
```

---

### 💡 풀이

- 첫 번째 예제에서처럼 **headgear**가 2개이고 **eyewear**가 1개일 때 가능한 조합의 수는 5개라고 되어있습니다.
- 이 조합은 아래와 같이 분리할 수 있습니다.
	- 머리 : X, headgear1, headgear2
	- 얼굴 : X, eyewear1
- (headgear1, X), (headgear1, eyewear1), (headgear2, X), (headgear2, eyewear1), (X, eyewear1), (X, X)
- 문제의 조건에서 아무것도 입지 않는 경우는 없다고 했으니 마지막 조건만 빼주면 되겠죠!
- 결국 **입지 않는 경우를 포함한 의상의 종류의 곱 - 1** 이 조합의 수가 되는 것입니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(String[][] clothes) {
        HashMap<String, Integer> map = new HashMap<>();
        
        for (String[] el : clothes) {
            String type = el[1];
            map.put(type, map.getOrDefault(type, 1) + 1);
        }
        
        int answer = 1;
        for (String key : map.keySet()) {
            answer *= map.get(key);
        }
        
        return answer - 1;
    }
}
```

- 해시맵에 의상의 종류를 넣어줍니다.
- getOrDefault는 map에 해당 키가 있다면 값을 가져오고 없다면 default값으로 넣는다는 의미입니다.
- map.getOrDefault(type, 1) 이 default 값을 1로 지정한다는 의미이고 해당 키가 이미 들어있다면 +1이 되면서 입지 않은 경우를 포함한 개수가 만들어지겠죠!
- 이 때 getOrDefault의 default 값으로 1이 들어가면 그것으로 끝이 아니라 +1 연산도 한다는 것을 기억해야 합니다.

| type     | count |
|:-------- |:----- |
| headgear | 3     |
| eyewear  | 2      |

- 결국 해시맵은 위와 같은 구조를 갖게 됩니다.