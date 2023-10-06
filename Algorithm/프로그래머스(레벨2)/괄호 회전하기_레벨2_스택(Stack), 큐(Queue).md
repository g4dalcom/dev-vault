# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/76502)

## 📝 문제

다음 규칙을 지키는 문자열을 올바른 괄호 문자열이라고 정의합니다.

- `()`, `[]`, `{}` 는 모두 올바른 괄호 문자열입니다.
- 만약 `A`가 올바른 괄호 문자열이라면, `(A)`, `[A]`, `{A}` 도 올바른 괄호 문자열입니다. 예를 들어, `[]` 가 올바른 괄호 문자열이므로, `([])` 도 올바른 괄호 문자열입니다.
- 만약 `A`, `B`가 올바른 괄호 문자열이라면, `AB` 도 올바른 괄호 문자열입니다. 예를 들어, `{}` 와 `([])` 가 올바른 괄호 문자열이므로, `{}([])` 도 올바른 괄호 문자열입니다.

대괄호, 중괄호, 그리고 소괄호로 이루어진 문자열 `s`가 매개변수로 주어집니다. 이 `s`를 왼쪽으로 x (_0 ≤ x < (`s`의 길이)_) 칸만큼 회전시켰을 때 `s`가 올바른 괄호 문자열이 되게 하는 x의 개수를 return 하도록 solution 함수를 완성해주세요.

---

##### 제한사항

- s의 길이는 1 이상 1,000 이하입니다.

---

##### 입출력 예

|s|result|
|---|---|
|`"[](){}"`|3|
|`"}]()[{"`|2|
|`"[)(]"`|0|
|`"}}}"`|0|

---

##### 입출력 예 설명

**입출력 예 #1**

- 다음 표는 `"[](){}"` 를 회전시킨 모습을 나타낸 것입니다.

|x|s를 왼쪽으로 x칸만큼 회전|올바른 괄호 문자열?|
|---|---|---|
|0|`"[](){}"`|O|
|1|`"](){}["`|X|
|2|`"(){}[]"`|O|
|3|`"){}[]("`|X|
|4|`"{}[]()"`|O|
|5|`"}[](){"`|X|

- 올바른 괄호 문자열이 되는 x가 3개이므로, 3을 return 해야 합니다.

**입출력 예 #2**

- 다음 표는 `"}]()[{"` 를 회전시킨 모습을 나타낸 것입니다.

|x|s를 왼쪽으로 x칸만큼 회전|올바른 괄호 문자열?|
|---|---|---|
|0|`"}]()[{"`|X|
|1|`"]()[{}"`|X|
|2|`"()[{}]"`|O|
|3|`")[{}]("`|X|
|4|`"[{}]()"`|O|
|5|`"{}]()["`|X|

- 올바른 괄호 문자열이 되는 x가 2개이므로, 2를 return 해야 합니다.

**입출력 예 #3**

- s를 어떻게 회전하더라도 올바른 괄호 문자열을 만들 수 없으므로, 0을 return 해야 합니다.

**입출력 예 #4**

- s를 어떻게 회전하더라도 올바른 괄호 문자열을 만들 수 없으므로, 0을 return 해야 합니다.

---

### 💡 풀이

- 로직은 간단한데 조건문을 작성하는 게 매우 까다로웠습니다 ㅠㅠ 좀 더 깔끔하게 작성하고 싶은데 무언가 아쉬운 코드입니다..
- 우선은 문제의 핵심은 **괄호를 왼쪽으로 회전하는 것** 과 **괄호쌍이 올바른지 확인하는 것** 입니다.
- 괄호를 왼쪽으로 회전하는 것은 Queue에서 빼고 넣으면 자연스럽게 구현이 됩니다. `q.add(q.poll());`
- 그리고 괄호가 올바른지 확인을 하는 것은 Stack을 이용하면 됩니다. 기본적으로 스택에 열린 괄호들('(', '\[', '{') 을 넣고 닫힌 괄호가 나왔다면 스택의 맨 위의 요소가 닫힌 괄호와 맞는 쌍인지를 확인하면 됩니다.

```java
if (closedBracket.contains(current)) {
	if (stack.isEmpty()) isCorrect = false;
	else if (map.get(stack.peek()) == current) {
		stack.pop();
	} else isCorrect = false;
} else {
	stack.push(current);
}
```

- 닫힌 괄호일 때 스택이 비어있다면 쌍을 맞춰볼 열린 괄호가 없다는 뜻이므로 올바른 괄호쌍이 아니겠죠.
- 그리고 괄호를 왼쪽으로 회전시키는 작업은 문자열의 길이만큼 하게 됩니다.

```java
int roop = s.length();

while (roop-- > 0) {
	if (correctBrackets()) {
		count++;
	}
	
	q.add(q.poll());
}
```

- 주어진 문자열부터 확인하고 그 후 최초 문자열로 돌아오기 직전까지 확인을 해야하니까요!
- 또한 저같은 경우는 문자열이 저장된 Queue를 그대로 넘겨서 확인을 했는데 Queue에 있는 문자를 하나씩 꺼내면서 스택에 넣고, 비교하는 작업을 하다보면 처음 Queue와 다르게 뒤죽박죽 될 여지가 있습니다.
- 그래서 Queue의 요소를 빼고 넣고를 Queue의 길이만큼 해주었습니다. 큐에 ABCD가 들어있다면 `q.add(q.poll())` 을 네 번 해주면 그대로 ABCD로 돌아오겠죠!
- 대신 이 작업 때문에 탐색 중간에 잘못된 괄호쌍이라는 것을 알아도 q.size() 만큼의 for문은 돌아주어야 했습니다.

```java
boolean isCorrect = true;

if (closedBracket.contains(q.peek())) return false;
        
for (int i = 0; i < q.size(); i++) {
	char current = q.poll();
	q.add(current);
	
	if (isCorrect) {
		if (closedBracket.contains(current)) {
			if (stack.isEmpty()) isCorrect = false;
			else if (map.get(stack.peek()) == current) {
				stack.pop();
			} else isCorrect = false;
		} else {
			stack.push(current);
		}
	}
}

if (!stack.isEmpty()) isCorrect = false;

return isCorrect;
```

- 위와 같이 isCorrect라는 boolean 값을 이용하였습니다.
- 괄호를 회전시키기 쉬운 방법이 큐라고 생각해서 큐를 이용하였는데 뒤로 갈 수록 큐 때문에 조건문 작성이 지저분해지고, boolean 을 이용해야 하는 등 제약 사항이 발생해서 개인적으로는 좀 아쉬운 풀이라고 생각합니다 ㅠㅠ

### 🔍 정답

```java
import java.util.*;

class Solution {
    static Queue<Character> q = new LinkedList<>();
    static Map<Character, Character> map = Map.of('[', ']', '(', ')', '{', '}');
    static List<Character> closedBracket = List.of(')', '}', ']');
    
    public int solution(String s) {    
        for (int i = 0; i < s.length(); i++) {
            q.add(s.charAt(i));
        }
        
        int count = 0;
        int roop = s.length();
        while (roop-- > 0) {
            if (correctBrackets()) {
                count++;
            }
            
            q.add(q.poll());
        }
        
        return count;
    }
    
    public boolean correctBrackets() {
        Stack<Character> stack = new Stack<>();
        boolean isCorrect = true;
        
        if (closedBracket.contains(q.peek())) return false;
        
        for (int i = 0; i < q.size(); i++) {
            char current = q.poll();
            q.add(current);
            
            if (isCorrect) {
                if (closedBracket.contains(current)) {
                    if (stack.isEmpty()) isCorrect = false;
                    else if (map.get(stack.peek()) == current) {
                        stack.pop();
                    } else isCorrect = false;
                } else {
                    stack.push(current);
                }
            }
        }
        
        if (!stack.isEmpty()) isCorrect = false;
        
        return isCorrect;
    }
}
```