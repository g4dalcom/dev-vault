# [문제링크]()

## 📝 문제

Given a string `s`, return _the longest_ _palindromic__substring_ in `s`.

**Example 1:**

**Input:** s = "babad"
**Output:** "bab"
**Explanation:** "aba" is also a valid answer.

**Example 2:**

**Input:** s = "cbbd"
**Output:** "bb"

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consist of only digits and English letters.

---

### 💡 풀이

- 팰린드롬 문자는 두 가지로 나눌 수가 있습니다.
- aba 처럼 가운데 하나의 문자를 두고 양쪽이 같은 문자열과 bb 처럼 데칼코마니 형식의 문자열입니다.
- aba처럼 가운데 하나의 문자가 있는 경우는 현재 인덱스 i 를 기준으로 i-1 과 i+1이 같은지를 확인하고 같다면 그 때부터 팰린드롬 여부를 판별합니다. 판별은 left(i-1) 포인터는 왼쪽으로, right(i+1) 포인터는 오른쪽으로 한 칸씩 이동하며 문자가 같은지 확인하는 것이죠!
- bb처럼 데칼코마니 형식은 현재 인덱스 i를 기준으로 i+1이 같다면 팰린드롬 여부를 판별합니다. 이 때에도 두 개의 포인터(left, right)를 이용합니다.
- 팰린드롬 여부를 판별하는 로직은 동일하기 때문에 하나의 메소드로 빼면서 매개변수에 차이를 두었습니다.

### 🔍 정답

```java
class Solution {
    static int max;
    static int start;
    static int end;

    public String longestPalindrome(String s) {
        if (s.length() == 1) return s;

        max = 0;
        start = 0;
        end = 0;
        for (int i = 0; i < s.length()-1; i++) {
            if (i != 0 && s.charAt(i-1) == s.charAt(i+1)) {
                checkPalindrom(i-1, i+1, s);
            }
            if (s.charAt(i) == s.charAt(i+1)) {
                checkPalindrom(i, i+1, s);
            }
        }
        
        return s.substring(start, end+1);
    }

    public void checkPalindrom(int left, int right, String s) {
        while (left >= 0 && right < s.length()) {
            if (s.charAt(left) != s.charAt(right)) break;

            if (max < right - left + 1) {
                start = left;
                end = right;
                max = right - left + 1;
            }

            left--;
            right++;
        }
    }
}
```