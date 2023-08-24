# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/17677)

## 📝 문제

## 뉴스 클러스터링

여러 언론사에서 쏟아지는 뉴스, 특히 속보성 뉴스를 보면 비슷비슷한 제목의 기사가 많아 정작 필요한 기사를 찾기가 어렵다. Daum 뉴스의 개발 업무를 맡게 된 신입사원 튜브는 사용자들이 편리하게 다양한 뉴스를 찾아볼 수 있도록 문제점을 개선하는 업무를 맡게 되었다.

개발의 방향을 잡기 위해 튜브는 우선 최근 화제가 되고 있는 "카카오 신입 개발자 공채" 관련 기사를 검색해보았다.

- 카카오 첫 공채..'블라인드' 방식 채용
- 카카오, 합병 후 첫 공채.. 블라인드 전형으로 개발자 채용
- 카카오, 블라인드 전형으로 신입 개발자 공채
- 카카오 공채, 신입 개발자 코딩 능력만 본다
- 카카오, 신입 공채.. "코딩 실력만 본다"
- 카카오 "코딩 능력만으로 2018 신입 개발자 뽑는다"

기사의 제목을 기준으로 "블라인드 전형"에 주목하는 기사와 "코딩 테스트"에 주목하는 기사로 나뉘는 걸 발견했다. 튜브는 이들을 각각 묶어서 보여주면 카카오 공채 관련 기사를 찾아보는 사용자에게 유용할 듯싶었다.

유사한 기사를 묶는 기준을 정하기 위해서 논문과 자료를 조사하던 튜브는 "자카드 유사도"라는 방법을 찾아냈다.

자카드 유사도는 집합 간의 유사도를 검사하는 여러 방법 중의 하나로 알려져 있다. 두 집합 `A`, `B` 사이의 자카드 유사도 `J(A, B)`는 두 집합의 교집합 크기를 두 집합의 합집합 크기로 나눈 값으로 정의된다.

예를 들어 집합 `A` = {1, 2, 3}, 집합 `B` = {2, 3, 4}라고 할 때, 교집합 `A ∩ B` = {2, 3}, 합집합 `A ∪ B` = {1, 2, 3, 4}이 되므로, 집합 `A`, `B` 사이의 자카드 유사도 `J(A, B)` = 2/4 = 0.5가 된다. 집합 A와 집합 B가 모두 공집합일 경우에는 나눗셈이 정의되지 않으니 따로 `J(A, B)` = 1로 정의한다.

자카드 유사도는 원소의 중복을 허용하는 다중집합에 대해서 확장할 수 있다. 다중집합 `A`는 원소 "1"을 3개 가지고 있고, 다중집합 `B`는 원소 "1"을 5개 가지고 있다고 하자. 이 다중집합의 교집합 `A ∩ B`는 원소 "1"을 min(3, 5)인 3개, 합집합 `A ∪ B`는 원소 "1"을 max(3, 5)인 5개 가지게 된다. 다중집합 `A` = {1, 1, 2, 2, 3}, 다중집합 `B` = {1, 2, 2, 4, 5}라고 하면, 교집합 `A ∩ B` = {1, 2, 2}, 합집합 `A ∪ B` = {1, 1, 2, 2, 3, 4, 5}가 되므로, 자카드 유사도 `J(A, B)` = 3/7, 약 0.42가 된다.

이를 이용하여 문자열 사이의 유사도를 계산하는데 이용할 수 있다. 문자열 "FRANCE"와 "FRENCH"가 주어졌을 때, 이를 두 글자씩 끊어서 다중집합을 만들 수 있다. 각각 {FR, RA, AN, NC, CE}, {FR, RE, EN, NC, CH}가 되며, 교집합은 {FR, NC}, 합집합은 {FR, RA, AN, NC, CE, RE, EN, CH}가 되므로, 두 문자열 사이의 자카드 유사도 `J("FRANCE", "FRENCH")` = 2/8 = 0.25가 된다.

### 입력 형식

- 입력으로는 `str1`과 `str2`의 두 문자열이 들어온다. 각 문자열의 길이는 2 이상, 1,000 이하이다.
- 입력으로 들어온 문자열은 두 글자씩 끊어서 다중집합의 원소로 만든다. 이때 영문자로 된 글자 쌍만 유효하고, 기타 공백이나 숫자, 특수 문자가 들어있는 경우는 그 글자 쌍을 버린다. 예를 들어 "ab+"가 입력으로 들어오면, "ab"만 다중집합의 원소로 삼고, "b+"는 버린다.
- 다중집합 원소 사이를 비교할 때, 대문자와 소문자의 차이는 무시한다. "AB"와 "Ab", "ab"는 같은 원소로 취급한다.

### 출력 형식

입력으로 들어온 두 문자열의 자카드 유사도를 출력한다. 유사도 값은 0에서 1 사이의 실수이므로, 이를 다루기 쉽도록 65536을 곱한 후에 소수점 아래를 버리고 정수부만 출력한다.

### 예제 입출력

|str1|str2|answer|
|---|---|---|
|FRANCE|french|16384|
|handshake|shake hands|65536|
|aa1+aa2|AAAA12|43690|
|E=M*C^2|e=m*c^2|65536|

---

### 💡 풀이

- 처음부터 문자열을 가지고 생각하면 복잡하니 숫자를 가지고 생각을 해봅시다.
- str1 = \[1, 1, 2, 2, 3\], str2 = \[1, 2, 2, 4, 5\] 인 경우
	- 교집합(intersection) = \[1, 2, 2\]
	- 합집합(union) = \[1, 1, 2, 2, 3, 4, 5\]
- 위에서 조건을 찾아보면 **교집합에 들어가는 조건은 두 배열에 모두 존재하는 개수 중 적은 값(Math.min)** 이고 **합집합에 들어가는 조건은 두 배열에 모두 존재하는 개수 중 많은 값(Math.max) 이거나 하나의 배열에만 존재하는 경우**입니다.
- 추가로 만약 **두 배열에 모두 존재하면서 개수도 같다면 교집합과 합집합에 모두** 들어가게 됩니다.

<br />

- 구해야하는 값은 교집합의 개수와 합집합의 개수입니다.
- 각 문자열의 길이는 최대 1,000이므로 최악의 경우는 999개짜리 배열이 2개가 필요할 수도 있습니다. 이 배열이든 리스트든 순회를 한다고 하면 효율이 좋지 않을 것이라고 생각했고
- 해시맵에 각 문자열을 key로 저장하고 getOrDefault 메소드로 중복인 문자열을 하나의 키로 묶어서 value값을 세어주고 containsKey 메소드를 이용해서 두 해시맵의 key값을 비교하기로 하였습니다.

```java
HashMap<String, Integer> map1 = new HashMap<>();
HashMap<String, Integer> map2 = new HashMap<>();

// 문자열 두 자리씩 끊기 + 문자열로만 이루어졌다면 해시맵에 추가
public void partition(String str, HashMap<String, Integer> map) {
        String element = "";
        char prev = str.charAt(0);
        for (int i = 1; i < str.length(); i++) {
            element = prev + Character.toString(str.charAt(i));
            element = element.toLowerCase();
            if (check(element)) {
                map.put(element, map.getOrDefault(element, 0) + 1);
            }
            prev = str.charAt(i);
        }
    }

// 문자열로만 이루어졌는지 체크
public boolean check(String str) {
	str = str.toLowerCase();
	for (int i = 0; i < str.length(); i++) {
		if (str.charAt(i) < 97 || str.charAt(i) > 122) return false;
	}
	return true;
}
```

- 먼저 해시맵 두 개를 선언해주고 문자열을 두 개씩 끊어줍니다. 이 때 공백이나 특수문자, 숫자가 들어가있지 않은지 확인(check 메소드)하고 문자열로만 이루어져있다면 해시맵에 넣어줍니다.
- 문자열로만 이루어졌는지 확인하는 작업은 아스키 코드를 이용해서 a(97) ~ z(122) 사이에 있는지를 확인했습니다.

```java
ArrayList<String> removeList = new ArrayList<>();

for (String key : map1.keySet()) {
            if (map2.containsKey(key)) {
                if (map1.get(key).equals(map2.get(key))) {
                    union += map1.get(key);
                    intersection += map1.get(key);
                } else {
                    union += Math.max(map1.get(key), map2.get(key));
                    intersection += Math.min(map1.get(key), map2.get(key));
                }
            } else union += map1.get(key);
            removeList.add(key);
        }
```

- 그 다음 작업은 map1을 기준으로 map2를 확인하는 것입니다.
- 맨 처음 구했던 조건을 구현하는 작업이라고 보면 됩니다.
- map1과 map2에 모두 존재하면서 개수가 같다면 교집합, 합집합에 모두 추가하고 개수가 다르다면 적은 개수는 교집합에, 많은 개수는 합집합에 추가해줍니다.
- 그리고 만약 map1에만 존재한다면 합집합에 추가해주면 되겠죠!
- 작업이 끝나면 해시맵에서 해당하는 key는 삭제를 해주어야 합니다. 그런데 keySet()으로 루프를 도는 동안 해시맵에서 요소를 삭제하려고 하면 **java.util.concurrentmodificationexception** 가 발생합니다.
- 이것은 Collection을 순회하는 도중에 해당 객체를 변경할 때 발생하는 것인데 이것을 해결하기 위해 따로 List를 선언해서 리스트에 지울 값을 넣어주었습니다.

```java
for (String i : removeList) {
	map1.remove(i);
	map2.remove(i);
}
```

- 그리고 순회가 끝난 후에 지워주는 것이죠!
- map1을 기준으로 루프를 돌았으니 map1은 모두 비워질 것이고
- map2에는 map1과 겹치지 않는 key들만 남을 것입니다.

```java
for (String key : map2.keySet()) {
	union += map2.get(key);
}
```

- 겹치지 않는 문자열은 합집합에 추가해주면 되니까 그대로 구현해줍니다!

```java
if (intersection == 0 && union == 0) return BASE_NUM;
        
return (int) ((double) intersection / union * 65536);
```

- 마지막으로 문제에서 두 집합 모두 공집합일 때 나눗셈이 정의되지 않으므로 1로 정의한다고 했으니 그것도 그대로 구현해줍니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(String str1, String str2) {
        final int BASE_NUM = 65536;
        int union = 0;
        int intersection = 0;

        HashMap<String, Integer> map1 = new HashMap<>();
        HashMap<String, Integer> map2 = new HashMap<>();

        ArrayList<String> removeList = new ArrayList<>();

        partition(str1, map1);
        partition(str2, map2);

        for (String key : map1.keySet()) {
            if (map2.containsKey(key)) {
                if (map1.get(key).equals(map2.get(key))) {
                    union += map1.get(key);
                    intersection += map1.get(key);
                } else {
                    union += Math.max(map1.get(key), map2.get(key));
                    intersection += Math.min(map1.get(key), map2.get(key));
                }
            } else union += map1.get(key);
            removeList.add(key);
        }

        for (String i : removeList) {
            map1.remove(i);
            map2.remove(i);
        }

        for (String key : map2.keySet()) {
            union += map2.get(key);
        }
        
        if (intersection == 0 && union == 0) return BASE_NUM;
        
        return (int) ((double) intersection / union * 65536);
        
    }
    
    public void partition(String str, HashMap<String, Integer> map) {
        String element = "";
        char prev = str.charAt(0);
        for (int i = 1; i < str.length(); i++) {
            element = prev + Character.toString(str.charAt(i));
            element = element.toLowerCase();
            if (check(element)) {
                map.put(element, map.getOrDefault(element, 0) + 1);
            }
            prev = str.charAt(i);
        }
    }
    
    public boolean check(String str) {
        str = str.toLowerCase();
        for (int i = 0; i < str.length(); i++) {
            if (str.charAt(i) < 97 || str.charAt(i) > 122) return false;
        }
        return true;
    }
}
```