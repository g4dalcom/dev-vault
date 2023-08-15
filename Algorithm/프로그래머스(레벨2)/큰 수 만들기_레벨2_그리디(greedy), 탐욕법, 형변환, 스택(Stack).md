# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42883)

## 📝 문제

어떤 숫자에서 k개의 수를 제거했을 때 얻을 수 있는 가장 큰 숫자를 구하려 합니다.

예를 들어, 숫자 1924에서 수 두 개를 제거하면 [19, 12, 14, 92, 94, 24] 를 만들 수 있습니다. 이 중 가장 큰 숫자는 94 입니다.

문자열 형식으로 숫자 number와 제거할 수의 개수 k가 solution 함수의 매개변수로 주어집니다. number에서 k 개의 수를 제거했을 때 만들 수 있는 수 중 가장 큰 숫자를 문자열 형태로 return 하도록 solution 함수를 완성하세요.

##### 제한 조건

- number는 2자리 이상, 1,000,000자리 이하인 숫자입니다.
- k는 1 이상 `number의 자릿수` 미만인 자연수입니다.

##### 입출력 예

|number|k|return|
|---|---|---|
|"1924"|2|"94"|
|"1231234"|3|"3234"|
|"4177252841"|4|"775841"|

---

### 💡 풀이

- 문제만 보면 간단해보였는데 생각을 많이 해야했던 문제였습니다.
- 주어진 문자열의 순서를 바꾸지 않으면서 가장 큰 수를 만들어야 하기 때문에 정렬을 사용할 수 없습니다.
- 우선 가장 큰 수가 되려면 첫째로 **최대 자릿수**를 만족해야합니다. 최대 자릿수는 **문자열 길이 - k**가 되겠죠!
- number = 4177252841, k = 4를 예시로 들면, 최대 자릿수는 6자리가 됩니다.
- 만약에 처음에 하나의 수를 선택한다면 최대 자릿수를 만족하기 위해서는 최소한 뒤에 남은 수가 5자리가 되어야 합니다.
- 이런 논리로 수를 하나 선택할 때마다 보장된 자릿수를 하나씩 줄여나가고 탐색 범위는 이전에 선택된 수 다음부터 보장된 자릿수 이전까지를 반복하면 될 것이라고 생각하였습니다.
- 말로는 굉장히 복잡해보이는데 정리를 하면 아래와 같습니다.

| 탐색범위 | 보장된자릿수 | 보장범위 | 수식         |
|:-------- |:------------ |:-------- |:-------- |
| 41772    | 5자리        | 52841    | length-5 |
| 725      | 4자리        | 2841     | length-4 |
| 252      | 3자리        | 841      | length-3 |
| 528      | 2자리        | 41       | length-2 |
| 41       | 1자리        | 1        | lenght-1 |

### 🔍 정답

```java
class Solution {
    public String solution(String number, int k) {
        StringBuilder sb = new StringBuilder();
        int start = 0;
        int end = number.length() - k;
        int index = 0;
        
        while (end-- > 0) {
            int max = 0;
            for (int i = start; i < number.length() - end; i++) {
                if (max < number.charAt(i) - '0') {
                    max = number.charAt(i) - '0';
                    index = i;
                }
            }
            sb.append(String.valueOf(max));
            start = index + 1;
        }
        return sb.toString();
    }
}
```


### 🔍 다른 정답(스택)

```java
import java.util.Stack;
class Solution {
    public String solution(String number, int k) {
        char[] result = new char[number.length() - k];
        Stack<Character> stack = new Stack<>();

        for (int i=0; i<number.length(); i++) {
            char c = number.charAt(i);
            while (!stack.isEmpty() && stack.peek() < c && k-- > 0) {
                stack.pop();
            }
            stack.push(c);
        }
        for (int i=0; i<result.length; i++) {
            result[i] = stack.get(i);
        }
        return new String(result);
    }
}
```

- 차례대로 스택에 넣다가 이전 수보다 큰 수를 만나면 스택에서 크거나 같은 수를 만날 때까지 pop()을 하는 방법입니다.
- 위 작업을 할 때마다 제거해야 하는 수(k)를 하나씩 줄여서 결과적으로 k = 0 이 되었을 때 종료하게 됩니다.