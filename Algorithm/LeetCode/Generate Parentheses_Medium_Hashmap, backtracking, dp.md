# [문제링크](https://leetcode.com/problems/generate-parentheses/)

## 📝 문제

Given `n` pairs of parentheses, write a function to _generate all combinations of well-formed parentheses_.

**Example 1:**

**Input:** n = 3
**Output:** ["((()))","(()())","(())()","()(())","()()()"]

**Example 2:**

**Input:** n = 1
**Output:** ["()"]

**Constraints:**

- `1 <= n <= 8`

---

### 💡 풀이

- n의 개수만큼 각각 여는 괄호, 닫는 괄호가 존재하고 이 괄호들을 가지고 올바른 괄호쌍을 만드는 문제입니다.
- 괄호쌍은 최대 8개까지만 존재할 수 있어서 백트래킹을 활용한 완전 탐색으로 구현해보았습니다.
- generate 메소드는 모든 괄호쌍을 재귀를 이용해 만드는 함수이고 isProperPair 메소드에서 올바른 괄호쌍 여부를 판단해서 리스트에 담는 작업을 합니다.
- 좀 더 간결한 다른 풀이는 올바른 괄호의 규칙성을 추가하여 **가지치기** 를 한 풀이인데요!
- n = 3일 때 가능한 괄호쌍은 \["((()))","(()())","(())()","()(())","()()()"\] 이고 여기서 규칙을 찾을 수 있습니다.
- 우선 괄호쌍이 n개만큼 각각 있다고 정의합니다. 그러면 여는 괄호("(") 3개, 닫는 괄호(")") 3개가 초기값이겠죠?! 이 때는 여는 괄호만이 올 수가 있습니다. 그리고 "()" 상태에서는 괄호가 각각 2개씩 존재할텐데, 이 때에도 여는 괄호만이 올 수가 있습니다. 따라서 여기서 알 수 있는 규칙은 **여는 괄호와 닫는 괄호가 개수가 같다면 여는 괄호만 와야 한다는 것**입니다.
- 그리고 닫는 괄호는 여는 괄호보다 많아지는 경우가 없어야 하겠죠?!
- 이 두 가지 명확한 조건을 추가하면 훨씬 탐색의 범위가 줄어들게 됩니다!

### 🔍 정답

```java
import java.util.*;

class Solution {
    static HashMap<String, Integer> map;
    static List<String> result;
    static StringBuilder sb;
    static String[] parentheses = {"(", ")"};

    public List<String> generateParenthesis(int n) {
        result = new ArrayList<>();
        sb = new StringBuilder();
        map = new HashMap<>();
        map.put(parentheses[0], n);
        map.put(parentheses[1], n);

        generate(0, n * 2);

        return result;
    }

    public void generate(int depth, int size) {
        if (depth == size) {
            if (isProperPair()) {
                result.add(sb.toString());
            }
            return;
        }

        for (int i = 0; i < 2; i++) {
            if (map.get(parentheses[i]) > 0) {
                map.replace(parentheses[i], map.get(parentheses[i]) - 1);
                sb.append(parentheses[i]);
                generate(depth + 1, size);
                map.replace(parentheses[i], map.get(parentheses[i]) + 1);
                sb.deleteCharAt(sb.length() - 1);
            }
        }
    }

    public boolean isProperPair() {
        Queue<Character> q = new LinkedList<>();

        String pair = sb.toString();
        for (int i = 0; i < pair.length(); i++) {
            char parentheses = pair.charAt(i);

            if (q.isEmpty() && parentheses == ')') return false;

            if (parentheses == '(') {
                q.add(parentheses);
            } else {
                q.poll();
            }
        }

        return q.size() == 0;
    }
}
```


### 💡 다른 정답

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList<>();
        generate_parenthesis(ans,0,0,n,"");
        return ans;
    }
    public void generate_parenthesis(List<String> ans, int open, int close, int n, String str) {
        if (str.length()==2*n) {
            ans.add(str);
            return;
        }
        if (open<n) {
            generate_parenthesis(ans,open+1,close,n,str+"(");
        }
        if (close<open) {
            generate_parenthesis(ans,open,close+1,n,str+")");
        }
    }
}
```