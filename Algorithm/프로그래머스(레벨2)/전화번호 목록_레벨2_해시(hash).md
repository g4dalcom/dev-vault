# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42577)

## 📝 문제

전화번호부에 적힌 전화번호 중, 한 번호가 다른 번호의 접두어인 경우가 있는지 확인하려 합니다.  
전화번호가 다음과 같을 경우, 구조대 전화번호는 영석이의 전화번호의 접두사입니다.

- 구조대 : 119
- 박준영 : 97 674 223
- 지영석 : 11 9552 4421

전화번호부에 적힌 전화번호를 담은 배열 phone_book 이 solution 함수의 매개변수로 주어질 때, 어떤 번호가 다른 번호의 접두어인 경우가 있으면 false를 그렇지 않으면 true를 return 하도록 solution 함수를 작성해주세요.

##### 제한 사항

- phone_book의 길이는 1 이상 1,000,000 이하입니다.
    - 각 전화번호의 길이는 1 이상 20 이하입니다.
    - 같은 전화번호가 중복해서 들어있지 않습니다.

##### 입출력 예제

|phone_book|return|
|---|---|
|["119", "97674223", "1195524421"]|false|
|["123","456","789"]|true|
|["12","123","1235","567","88"]|false|

##### 입출력 예 설명

입출력 예 #1  
앞에서 설명한 예와 같습니다.

입출력 예 #2  
한 번호가 다른 번호의 접두사인 경우가 없으므로, 답은 true입니다.

입출력 예 #3  
첫 번째 전화번호, “12”가 두 번째 전화번호 “123”의 접두사입니다. 따라서 답은 false입니다.

---

### 💡 풀이

- 처음에는 문자열의 길이를 기준으로 생각하였습니다.
- 문자열의 길이가 길다는 것은 더 짧은 문자열의 접두사가 될 수 없다는 의미이기 때문에 문자열의 길이가 긴 것부터 비교해나가면 될 것이라고 생각하였습니다.
- 그래서 문자열의 길이를 기준으로 new Comparator 를 이용해서 내림차순 정렬한 후 ArrayList에 담아주었습니다.

> List에 담으면 인덱스를 이용한 삭제가 불가능하기 때문에 ArrayList에 담았습니다.


- 그런 후에 ArrayList의 마지막 요소(가장 짧은 요소)부터 remove를 하면서 ArrayList 내에 해당 요소로 시작하는 문자열이 있는지를 확인하였습니다.
- 이 로직은 효율성 테스트 3, 4번을 통과하지 못했습니다.

### ❌ 효율성 테스트 실패

```java
import java.util.*;

class Solution {
    public boolean solution(String[] phone_book) {
        Arrays.sort(phone_book, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o2.length() - o1.length();
            }
        });
        
        ArrayList<String> list = new ArrayList<>(Arrays.asList(phone_book));
        
        while (list.size() > 0) {
            String cur = list.remove(list.size() - 1);
            
            for (int i = 0; i < list.size(); i++) {
                if (list.get(i).startsWith(cur)) return false;
            }
        }
        
        return true;
    }
}
```

- 방법을 바꾸어서 정렬까지는 동일하나, ArrayList에 순차적으로 가장 긴 요소부터 넣어주면서 ArrayList 내에 조건에 맞는 문자열이 있는지를 체크하였습니다.
- 이 방법 또한 효율성 테스트 3, 4번을 통과하지 못했습니다.

### ❌ 효율성 테스트 실패

```java
import java.util.*;

class Solution {
    public boolean solution(String[] phone_book) {
        Arrays.sort(phone_book, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o2.length() - o1.length();
            }
        });
        
        ArrayList<String> list = new ArrayList<>();
        
        for (int i = 0; i < phone_book.length; i++) {
            for (int j = 0; j < list.size(); j++) {
                if (list.get(j).startsWith(phone_book[i])) return false;
            }
            
            list.add(phone_book[i]);
        }
        return true;
    }
}
```

- 처음 했던 방법은 순회를 거듭할 수록 탐색의 수가 적어지고 두 번째 방법은 반대로 탐색의 수가 점점 늘어나는 구조라서 크게 다르지 않은 로직이었던 것 같습니다.

- 사실 처음에 문자열의 길이를 기준으로 생각을 했던 것이 문제를 어렵게 접근하게 되었던 이유였습니다 ㅠㅠ
- 문자열 배열 자체를 그대로 정렬하면 사전순으로 정렬이 되기 때문에 직전의 값이 다음 값에 포함되지 않으면 당연히 그 다음 값에는 포함될 수가 없습니다.
	- "12", "1223", "123", "13", "139", "140"
- 따라서 모든 탐색을 할 필요없이 현재 문자열과 다음 문자열만 비교하면 되는 것입니다!

### 🔍 정답

```java
import java.util.*;

class Solution {
    public boolean solution(String[] phone_book) {
        Arrays.sort(phone_book);
        
        for (int i = 0; i < phone_book.length-1; i++) {
            if (phone_book[i+1].startsWith(phone_book[i])) return false;
        }
        
        return true;
    }
}
```


- 문제 유형이 Hash 여서 해시를 이용한 풀이도 찾아보았습니다. 
-  \["113", "11356"\] 가 있다면 1, 11, 1, 11, 113, 115 이런 식으로 substring을 하며 해시맵에 해당하는 key값이 있는지를 확인합니다.
- "11356".substring(0,3)이 "113"과 같으므로 접두사임을 판단할 수 있습니다.

### 💡 정답 (HashMap)

```java
import java.util.HashMap;
import java.util.Map;

class Solution {
    public boolean solution(String[] phoneBook) {
        boolean answer = true;

            Map<String, Integer> map = new HashMap<>();

            for(int i = 0; i < phoneBook.length; i++) {
                map.put(phoneBook[i], i);
            }

            for(int i = 0; i < phoneBook.length; i++) {
                for(int j = 0; j < phoneBook[i].length(); j++) {
                    if(map.containsKey(phoneBook[i].substring(0,j))) {
                        answer = false;
                        return answer;
                    }
                }
            }

            return answer;
    }
}
```