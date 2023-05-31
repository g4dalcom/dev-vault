Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return _a string of the words in reverse order concatenated by a single space._

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

**Example 1:**

**Input:** s = "the sky is blue"
**Output:** "blue is sky the"

**Example 2:**

**Input:** s = "  hello world  "
**Output:** "world hello"
**Explanation:** Your reversed string should not contain leading or trailing spaces.

**Example 3:**

**Input:** s = "a good   example"
**Output:** "example good a"
**Explanation:** You need to reduce multiple spaces between two words to a single space in the reversed string.

**Constraints:**

- `1 <= s.length <= 104`
- `s` contains English letters (upper-case and lower-case), digits, and spaces `' '`.
- There is **at least one** word in `s`.

**Follow-up:** If the string data type is mutable in your language, can you solve it **in-place** with `O(1)` extra space?


---

```java
class Solution {
    public String reverseWords(String s) {
        String[] str = s.strip().split(" ");
        
        StringBuilder sb = new StringBuilder();
        for (int i = str.length-1; i >= 0; i--) {
            if (!str[i].equals("")) {
                sb.append(str[i]);
                
                if (i != 0) sb.append(" ");
            }
        }
        return sb.toString();
    }
}
```

```java
public String reverseWords(String s) {
    String[] words = s.trim().split(" +");
    Collections.reverse(Arrays.asList(words));
    return String.join(" ", words);
}
```

정규표현식을 이용한 풀이
split(" +")는 둘 이상의 공백을 고려한다.
따라서 "the    sky is blue" 처럼 여러 칸의 공백이 있어도
\[the, sky, is blue\] 로 구현이 된다.


```java
class Solution {
    public String reverseWords(String s) {
        char[] ch = s.toCharArray();
        int range = ch.length;
        
        reverse(0, range-1, ch);
        reverseWord(range, ch);
        return cleanBlank(range, ch);
    }
    
    public void reverse(int start, int end, char[] ch) {
        while (start < end) {
            char temp = ch[start];
            ch[start++] = ch[end];
            ch[end--] = temp;
        }
    }
    
    public void reverseWord(int range, char[] ch) {
        int left = 0;
        int right = 0;
        
        while (left < range) {
            while (left < right || left < range && ch[left] == ' ') left++;
            while (right < left || right < range && ch[right] != ' ') right++;
            
            reverse(left, right-1, ch);
        }
    }
    
    public String cleanBlank(int range, char[] ch) {
        int pointer = 0;
        int idx = 0;
        
        while (pointer < range) {
            while (pointer < range && ch[pointer] == ' ') pointer++;
            while (pointer < range && ch[pointer] != ' ') ch[idx++] = ch[pointer++];
            while (pointer < range && ch[pointer] == ' ') pointer++;
            
            if (pointer < range) ch[idx++] = ' ';     
        }
        
        return new String(ch).substring(0, idx);
    }
}
```

투 포인터를 이용한 풀이
Step 1 : 문자열을 문자 배열로 바꾸어서 뒤집는다.
Step 2 : 단어별로 다시 뒤집는다.
Stop 3 : 공백을 제거한다.