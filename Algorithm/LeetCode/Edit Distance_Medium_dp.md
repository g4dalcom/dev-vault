# [문제링크](https://leetcode.com/problems/edit-distance/)

## 📝 문제

Given two strings `word1` and `word2`, return _the minimum number of operations required to convert `word1` to `word2`_.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character

**Example 1:**

**Input:** word1 = "horse", word2 = "ros"
**Output:** 3
**Explanation:** 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')

**Example 2:**

**Input:** word1 = "intention", word2 = "execution"
**Output:** 5
**Explanation:** 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')

**Constraints:**

- `0 <= word1.length, word2.length <= 500`
- `word1` and `word2` consist of lowercase English letters.

---

### 💡 풀이

- 최소 편집 거리 알고리즘을 이용하여 풀어야 하는 문제였습니다. 해당 알고리즘은 Levenshtein 알고리즘이라고도 불린다고 합니다.
- 2차원 배열을 이용해 문자열의 최소 변경 횟수를 구할 수가 있는데 방법이 정말 신박합니다.
- 우선 기본적으로 **편집 거리** 라는 단어를 사용하는데, 이것은 A라는 위치에서 B라는 문자를 만들기 위한 단계를 표기합니다.
- 예를 들어서 a 에서 ab 라는 문자를 만들기 위해서는 b라는 문자를 추가하는 한 단계의 작업이 필요하기 때문에 둘 사이의 거리는 1이 됩니다. a에서 aac 를 만든다면 a와 c를 각각 추가해야 하므로 거리는 2가 되겠죠?!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbN8FmI%2FbtsCAO3RsNg%2FKjx5BgWwRhCceHQLbW8Js0%2Fimg.png)

- 예제를 가지고 2차원 배열을 만들어보면 위와 같습니다. 열(horse)에서 행(ros)을 만드는 작업이라고 보고 이해를 하면 쉬울 것입니다.
- 우선은 아무것도 없는 상태(공집합)의 거리를 0이라고 두고 그 거리에서 각각 거리를 구해줍니다. 공집합에서 h까지의 편집 거리, ho 까지의 거리, hor 까지의 거리.. 이런식으로 거리는 1씩 늘어나게 될 것입니다.
- 이 상태에서 문자열을 비교하게 되는데 비교하는 글자가 동일하다면 이전 대각선 값을 그대로 가져오고(녹색 좌표)
- 다르다면 왼쪽, 왼쪽위, 위쪽의 세 값 중 가장 작은 값 + 1을 넣어주는 것입니다.
- 이 때 왼쪽은 삽입, 왼쪽위는 치환, 위쪽은 삭제 작업을 나타냅니다.
- ho 에서 ro가 되기 위한 과정을 보면, h에서 r이 되기 위해 치환 작업을 거쳐서 1이라는 편집 거리를 가졌고, 이제 ho 에서 ro가 될 때는 o 부분은 건드릴 필요가 없습니다. 따라서 h에서 r의 편집 거리 부분을 그대로 가져오면 되므로 현재 비교 문자가 동일하다면 이전 대각선 값을 그대로 가져오면 되는 것입니다.

### 🔍 정답

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int[][] dp = new int[word1.length()+1][word2.length()+1];

        for (int i = 1; i < dp.length; i++) {
            dp[i][0] = i;
        }

        for (int i = 1; i < dp[0].length; i++) {
            dp[0][i] = i;
        }

        for (int i = 1; i < dp.length; i++) {
            for (int j = 1; j < dp[0].length; j++) {
                if (word1.charAt(i-1) == word2.charAt(j-1)) dp[i][j] = dp[i-1][j-1];
                else dp[i][j] = Math.min(Math.min(dp[i-1][j], dp[i][j-1]), dp[i-1][j-1]) + 1;
            }
        }

        return dp[word1.length()][word2.length()];
    }
}
```