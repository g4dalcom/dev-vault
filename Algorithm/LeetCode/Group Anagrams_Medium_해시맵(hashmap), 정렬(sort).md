# [문제링크](https://leetcode.com/problems/group-anagrams/)

## 📝 문제

Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

**Input:** strs = ["eat","tea","tan","ate","nat","bat"]
**Output:** [["bat"],["nat","tan"],["ate","eat","tea"]]

**Example 2:**

**Input:** strs = [""]
**Output:** [[""]]

**Example 3:**

**Input:** strs = ["a"]
**Output:** [["a"]]

**Constraints:**

- `1 <= strs.length <= 104`
- `0 <= strs[i].length <= 100`
- `strs[i]` consists of lowercase English letters.

---

### 💡 풀이

- 해시맵과 정렬을 이용해 풀어보았습니다.
- \["eat","tea","tan","ate","nat","bat"\] 라는 배열에서 eat와 tea, ate는 정렬을 했을 때 모두 같은 aet가 됩니다. 이것을 이용해서 정렬했을 때의 문자열을 key로 갖는 해시맵을 만들었고
- 해시맵에 같은 키가 존재한다면 해당 key의 value인 인덱스에 문자열을 추가하도록 구현하였습니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        List<List<String>> result = new ArrayList<>();
        HashMap<String, Integer> map = new HashMap<>();

        int index = 0;
        for (int i = 0; i < strs.length; i++) {
            char[] ch = strs[i].toCharArray();
            Arrays.sort(ch);
            String current = new StringBuilder(new String(ch)).toString();

            if (!map.containsKey(current)) {
                map.put(current, index++);
                result.add(new ArrayList<>(Arrays.asList(strs[i])));
            } else {
                for (String key : map.keySet()) {
                    if (key.equals(current)) {
                        result.get(map.get(key)).add(strs[i]);
                    }
                }
            }
        }
        return result;
    }
}
```

### 💡 다른 정답

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<>();
        
        for (String word : strs) {
            char[] chars = word.toCharArray();
            Arrays.sort(chars);
            String sortedWord = new String(chars);
            
            if (!map.containsKey(sortedWord)) {
                map.put(sortedWord, new ArrayList<>());
            }
            
            map.get(sortedWord).add(word);
        }
        
        return new ArrayList<>(map.values());
    }
}
```

- 저와 완전히 동일한 접근 방법이지만 구현 방법에서 조금 차이가 있습니다. 해시맵의 API를 잘 이용한 풀이라고 생각하며 나름 공부가 되었습니다.
- 애초에 해시맵의 value를 List로 선언해서 value에 값을 추가하는 방식이고 return 때에도 map.values() 라는 메소드로 너무 깔끔하게 구현한 풀이네요!