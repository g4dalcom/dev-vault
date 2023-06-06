# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12909)

## 📝 문제

###### 문제 설명

괄호가 바르게 짝지어졌다는 것은 '(' 문자로 열렸으면 반드시 짝지어서 ')' 문자로 닫혀야 한다는 뜻입니다. 예를 들어

- "()()" 또는 "(())()" 는 올바른 괄호입니다.
- ")()(" 또는 "(()(" 는 올바르지 않은 괄호입니다.

'(' 또는 ')' 로만 이루어진 문자열 s가 주어졌을 때, 문자열 s가 올바른 괄호이면 true를 return 하고, 올바르지 않은 괄호이면 false를 return 하는 solution 함수를 완성해 주세요.

##### 제한사항

- 문자열 s의 길이 : 100,000 이하의 자연수
- 문자열 s는 '(' 또는 ')' 로만 이루어져 있습니다.

---

##### 입출력 예

|s|answer|
|---|---|
|"()()"|true|
|"(())()"|true|
|")()("|false|
|"(()("|false|

---

### 💡 풀이

Stack 의 기본과도 같은 문제이다.
참고로 입력값(s) 를 String\[\] 으로 바꿔서 풀면 효율성 테스트를 통과하지 못한다. 


### 🔍 정답

```java
import java.util.*;

class Solution {
    boolean solution(String s) {
        Stack<Character> stack = new Stack<>();
        
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);            
            
            if (ch == ')') {
                if (stack.isEmpty() || stack.peek() == ')') return false;
                else stack.pop();
            } else stack.push(ch);
        }
        
        return stack.isEmpty() ? true : false;
    }
}
```


### 💡 다른 정답

```java
class Solution {
    boolean solution(String s) {
        boolean answer = false;
        int count = 0;
        for(int i = 0; i<s.length();i++){
            if(s.charAt(i) == '('){
                count++;
            }
            if(s.charAt(i) == ')'){
                count--;
            }
            if(count < 0){
                break;
            }
        }
        if(count == 0){
            answer = true;
        }

        return answer;
    }
}
```

Stack을 쓰지 않고도 깔끔하게 해결한 답안이다!