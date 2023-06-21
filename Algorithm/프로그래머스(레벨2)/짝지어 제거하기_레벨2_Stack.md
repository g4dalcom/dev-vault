# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12973)

## 📝 문제

짝지어 제거하기는, 알파벳 소문자로 이루어진 문자열을 가지고 시작합니다. 먼저 문자열에서 같은 알파벳이 2개 붙어 있는 짝을 찾습니다. 그다음, 그 둘을 제거한 뒤, 앞뒤로 문자열을 이어 붙입니다. 이 과정을 반복해서 문자열을 모두 제거한다면 짝지어 제거하기가 종료됩니다. 문자열 S가 주어졌을 때, 짝지어 제거하기를 성공적으로 수행할 수 있는지 반환하는 함수를 완성해 주세요. 성공적으로 수행할 수 있으면 1을, 아닐 경우 0을 리턴해주면 됩니다.

예를 들어, 문자열 S = `baabaa` 라면

b _aa_ baa → _bb_ aa → _aa_ →

의 순서로 문자열을 모두 제거할 수 있으므로 1을 반환합니다.

##### 제한사항

- 문자열의 길이 : 1,000,000이하의 자연수
- 문자열은 모두 소문자로 이루어져 있습니다.

---

##### 입출력 예

|s|result|
|---|---|
|baabaa|1|
|cdcd|0|

##### 입출력 예 설명

입출력 예 #1  
위의 예시와 같습니다.  
입출력 예 #2  
문자열이 남아있지만 짝지어 제거할 수 있는 문자열이 더 이상 존재하지 않기 때문에 0을 반환합니다.

---

### 💡 풀이

- 스택 문제라는 것을 알면 쉽게 풀 수 있다!
- 문자열을 문자로 쪼개서 하나씩 스택에 추가하는데 만약 현재 문자와 스택의 맨 위 요소가 같다면 pop()을 하는 식이다.
- 모든 루프가 끝난 뒤 stack이 비어있다면 제대로 짝지어서 삭제가 된 것이고 그렇지 않다면 요건을 충족할 수 없는 것이 된다.


### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(String s) {
        Stack<Character> stack = new Stack<>();
        
        for (int i = 0; i < s.length(); i++) {
            if (!stack.isEmpty()) {
                char temp = stack.peek();
                
                if (temp == s.charAt(i)) {
                    stack.pop();
                    continue;
                }
            } 
            stack.push(s.charAt(i));
        }
        return stack.isEmpty() ? 1 : 0;
    }
}
```