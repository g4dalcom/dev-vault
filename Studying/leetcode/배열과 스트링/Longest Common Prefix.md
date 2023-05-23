Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

**Example 1:**

**Input:** strs = ["flower","flow","flight"]
**Output:** "fl"

**Example 2:**

**Input:** strs = ["dog","racecar","car"]
**Output:** ""
**Explanation:** There is no common prefix among the input strings.

**Constraints:**

-   `1 <= strs.length <= 200`
-   `0 <= strs[i].length <= 200`
-   `strs[i]` consists of only lowercase English letters.

---

```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < strs[0].length(); i++) {
            boolean flag = true;
            for (int j = 0; j < strs.length; j++) {
                if (i >= strs[j].length()) {
                    flag = false;
                    break;
                }

                if (strs[0].charAt(i) != strs[j].charAt(i)) {
                    flag = false;
                    break;
                }

            }
            if (flag) sb.append(strs[0].charAt(i));
            else return sb.toString();
        }
        
        return sb.toString();
    }
}
```


```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        Arrays.sort(strs);
        String s1 = strs[0];
        String s2 = strs[strs.length-1];
        int idx = 0;
        while(idx < s1.length() && idx < s2.length()){
            if(s1.charAt(idx) == s2.charAt(idx)){
                idx++;
            } else {
                break;
            }
        }
        return s1.substring(0, idx);
    }
}
```

- 정렬을 하면 알파벳 사전순으로 정렬이 된다.
	- \[cbyz, czabc, czabcd, czacae, czacbe, czal, czalmn\]
	- 이 정렬로 인해 공통 접두사가 있는 문자열을 함께 그룹화할 수 있다.
- 그 후 배열의 첫 번째 요소와 마지막 요소를 서로 비교해가면 모든 문자열이 공유하는 공통 접두사를 찾을 수 있다.