Given a string `s`, reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.

**Example 1:**

**Input:** s = "Let's take LeetCode contest"
**Output:** "s'teL ekat edoCteeL tsetnoc"

**Example 2:**

**Input:** s = "God Ding"
**Output:** "doG gniD"

**Constraints:**

- `1 <= s.length <= 5 * 104`
- `s` contains printable **ASCII** characters.
- `s` does not contain any leading or trailing spaces.
- There is **at least one** word in `s`.
- All the words in `s` are separated by a single space.

---

```java
class Solution {
    public String reverseWords(String s) {
        char[] ch = s.toCharArray();
        int left = 0;
        int right = 0;
        
        while (left < ch.length) {
            while (left < right || left < ch.length && ch[left] == ' ') left++;
            while (right < left || right < ch.length && ch[right] != ' ') right++;
            
            reverse(left, right-1, ch);
        }
        
        return String.valueOf(ch);
    }
    
    public void reverse(int start, int end, char[] ch) {
        while (start < end) {
            char temp = ch[start];
            ch[start++] = ch[end];
            ch[end--] = temp;
        }
    }
}
```