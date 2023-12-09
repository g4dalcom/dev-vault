# [문제링크](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

## 📝 문제

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](https://assets.leetcode.com/uploads/2022/03/15/1200px-telephone-keypad2svg.png)

**Example 1:**

**Input:** digits = "23"
**Output:** ["ad","ae","af","bd","be","bf","cd","ce","cf"]

**Example 2:**

**Input:** digits = ""
**Output:** []

**Example 3:**

**Input:** digits = "2"
**Output:** ["a","b","c"]

**Constraints:**

- `0 <= digits.length <= 4`
- `digits[i]` is a digit in the range `['2', '9']`.

---

### 💡 풀이

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fs1lFo%2FbtsBC1Xadr2%2F1hA0e8aGK0vkxC8nnRwlnK%2Fimg.png)

- 각 전화번호에 대응하는 알파벳 리스트가 있고 이 리스트들의 가능한 조합들을 출력하면 되는 문제입니다.
- 우선 리스트에 전화번호에 대응되는 알파벳 리스트를 입력해두었습니다. 리스트의 인덱스는 번호가 되고 해당 인덱스에 들어있는 리스트가 대응되는 알파벳들입니다. map.get(2) = \["a", "b", "c"\] 가 되는 것이죠!
- 그리고 주어지는 String 타입의 input을 하나하나의 int값으로 분리해준 뒤(int[] digit) 백트래킹을 하게 됩니다.
- 백트래킹의 종료 조건은 digit의 길이입니다. digit = \[2, 4, 6\] 이라면 "agm", "agn", "ago" . . . 이런 식으로 길이가 3이 될 때마다 result 에 저장을 해주면 되는 것이고
- 탐색 여부를 확인하기 위한 visit 배열도 필요하였습니다. visit 배열은 2차원 배열로 visit\[digit의 인덱스\]\[요소들의 인덱스\] 가 됩니다. digit = \[2, 4, 6\] 일 때 digit의 0번 인덱스인 2에 접근을 하기 위해서는 visit\[0\]\[0~2\] 가 되는 것입니다. visit\[0\]\[0\] = "a", visit\[0\]\[1\] = "b" 인 것이죠!

> 1차 배열에 digit값을 그대로 입력하는 방식도 생각해볼 수 있었는데(visit\[2\]\[0\] = "a", visit\[2\]\[1\] = "b" ...), 이 경우에는 사용하지 않는 인덱스까지 배열 선언이 된다는 점, input = "22" 처럼 동일한 숫자가 들어올 경우 탐색 여부를 제대로 가릴 수 없다는 점이 문제가 되었습니다.

- 그리고 백트래킹을 할 때는 depth로 인덱스를 추적하며 depth 가 digit의 길이가 될 때마다 result에 값을 저장해주는 식으로 구현하였습니다!

### 🔍 정답

```java
import java.util.*;

class Solution {
    static List<String> result;
    static List<List<String>> map;
    static int[] digit;
    static boolean[][] visit;
    static StringBuilder sb;

    public List<String> letterCombinations(String digits) {
        result = new ArrayList<>();
        map = new ArrayList<>();
        sb = new StringBuilder();
        digit = new int[digits.length()];

        if (digits.length() == 0) return result;

        for (int i = 0; i < 10; i++) {
            map.add(new ArrayList<>());
            map.get(i).addAll(mapping(i));
        }

        for (int i = 0; i < digits.length(); i++) {
            digit[i] = digits.charAt(i) - '0';
        }
        visit = new boolean[digit.length][4];

        combination(0);
        
        return result;
    }

    public void combination(int depth) {
        if (depth == digit.length) {
            result.add(sb.toString());
            return;
        }

        for (int i = 0; i < map.get(digit[depth]).size(); i++) {
            if (!visit[depth][i]) {
                visit[depth][i] = true;
                sb.append(map.get(digit[depth]).get(i));
                combination(depth+1);
                sb.delete(sb.length() - 1, sb.length());
                visit[depth][i] = false;
            }
        }
    }

    public List<String> mapping(int number) {
        List<String> list = new ArrayList<>();

        switch (number) {
            case 2: 
                list.addAll(Arrays.asList("a", "b", "c"));
                break;
            case 3:
                list.addAll(Arrays.asList("d", "e", "f"));
                break;
            case 4:
                list.addAll(Arrays.asList("g", "h", "i"));
                break;
            case 5:
                list.addAll(Arrays.asList("j", "k", "l"));
                break;
            case 6:
                list.addAll(Arrays.asList("m", "n", "o"));
                break;
            case 7:
                list.addAll(Arrays.asList("p", "q", "r", "s"));
                break;
            case 8:
                list.addAll(Arrays.asList("t", "u", "v"));
                break;
            case 9:
                list.addAll(Arrays.asList("w", "x", "y", "z"));
                break;
        }

        return list;
    }
}
```