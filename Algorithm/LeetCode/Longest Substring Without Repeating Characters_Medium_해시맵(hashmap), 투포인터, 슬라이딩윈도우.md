# [문제링크](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

## 📝 문제

Given a string `s`, find the length of the **longest** 

**substring**

 without repeating characters.

**Example 1:**

**Input:** s = "abcabcbb"
**Output:** 3
**Explanation:** The answer is "abc", with the length of 3.

**Example 2:**

**Input:** s = "bbbbb"
**Output:** 1
**Explanation:** The answer is "b", with the length of 1.

**Example 3:**

**Input:** s = "pwwkew"
**Output:** 3
**Explanation:** The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.

---

### 💡 풀이

- indexOf 를 이용해서 풀어보려고도 해보고 HashSet을 이용해보기도 하였는데 결국은 Character의 존재 여부와 해당 Character의 인덱스를 알아야 한다고 생각해서 Character를 key로, Index를 value로 하는 해시맵을 생성해서 풀이하였습니다.
- 처음에는 예제 케이스만 보고 굉장히 단순하게 접근을 했었습니다. 예제인 **abcabcbb** 를 예시로 들어서,
- 처음 a부터 b, c를 순차적으로 해시맵에 넣은 후, 3번 인덱스인 a를 넣으려는 순간 해시맵에 동일한 key가 있게 됩니다. 이 때 해시맵의 크기를 잰 후, 해시맵을 초기화하고 a부터 다시 넣는 것이죠.
- 해당 예제 케이스만 보면 맞는 풀이처럼 보일 수 있습니다. 그러나 **dvdf** 라는 케이스를 같은 방법으로 풀게 되면 어떻게 될까요?
- d와 v를 차례대로 넣고 다시 d를 넣으려고 하는데 key가 겹치네요! 해시맵의 길이를 재고 해시맵을 초기화한 후 다시 d부터 시작해보았습니다. 그러면 최종적으로 해시맵에는 d, f가 담기고 최대 길이는 2가 반환이 되겠죠?
- 그런데 사실 v, d, f 세 문자가 최대 길이가 되어야 합니다. 따라서 겹치는 문자가 나왔을 때 단순히 해시맵을 초기화하면 안 되는 것이죠.
- **dvedf** 라는 예제 케이스를 보면 더 명확해집니다. d, v, e 를 차례로 넣고 d를 넣을 때 초기화를 하면 d, f만 해시맵에 담기는데 사실은 v, e, d, f가 담겨야 합니다.
- **ckilbkd** 이 케이스는 어떨까요? c, k, i, l, b 를 담은 후에 k가 겹치는데 이 때도 단순히 해시맵을 초기화하고 k를 담으면 안 되겠죠? i, l, b, k, d 가 담기도록 구현해야 합니다.
- 여기서 알 수 있는 것은 겹치는 문자가 나왔을 때 해당 문자를 포함해서 이전 인덱스만 삭제하면 된다는 것이었습니다.
- **ckilbkd** 에서는 5번 인덱스 k를 넣으려고 할 때, 해시맵에 1번 인덱스 k가 존재하므로 1번 인덱스를 포함해 0번 인덱스 c까지 삭제를 하면 되는 것이죠.
- **dvedf** 과 **weewe** 의 예제 케이스도 모양은 조금씩 다르지만 같은 방식으로 구현했을 때 올바른 답을 낼 수가 있습니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int lengthOfLongestSubstring(String s) {
        HashMap<Character, Integer> map = new HashMap<>();
        int max = 0;

        for (int i = 0; i < s.length(); i++) {
            char current = s.charAt(i);
    
            if (map.containsKey(current)) {
                int index = map.get(current);

                for (Character key : Set.copyOf(map.keySet())) {
                    if (map.get(key) <= index) {
                        map.remove(key);
                    }
                }
            }
            map.put(current, i);
            max = Math.max(max, map.size());
        }
        return max;
    }
}
```

- 위 풀이에서 주의해야 할 점은,

```java
for (Character key : Set.copyOf(map.keySet())) {
	if (map.get(key) <= index) {
		map.remove(key);
	}
}
```

- 해시맵에서 해당 인덱스 이하의 문자를 삭제하는 로직입니다.
- 해시맵을 카피해서 루프를 돌았는데 카피하지 않으면 **ConcurrentModificationException** 이 발생합니다. Collection 객체를 순회하면서 동시에 객체를 수정(삭제) 하기 때문에 발생하는 문제인데요.
- 저처럼 복사본을 순회하면서 원본을 수정하는 방법을 사용해도 되고 멀티스레드 환경에서 동기화를 제공해주느 ConcurrentHashMap을 사용해도 됩니다.

### 💡 다른 정답(슬라이딩윈도우, 투포인터)

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int[] count = new int[128];
        int left = 0;
        int right = 0;
        int max = 0;

        while (right < s.length()) {
            char ch = s.charAt(right);

            if (count[ch] < 1) {
                count[ch]++;
                right++;
            } else {
                max = Math.max(max, right - left);
                while (s.charAt(left) != ch) {
                    count[s.charAt(left)]--;
                    left++;
                }
                left++;
                count[ch]--;
            }

            max = Math.max(max, right - left);
        }
        return max;
    }
}
```

- 기존 풀이와 접근 방식은 동일합니다. 그러나 해시맵을 이용해서 요소를 넣고 빼는 것이 아니라 두 개의 포인터(left, right)로 해당 로직을 구현했다는 점에 차이가 있습니다.
- 문자, 공백, 숫자, 기호 모두 들어올 수 있으므로 아스키 코드 전체인 128개를 배열로 선언해두고 문자가 존재하는지 여부를 판단하였습니다.