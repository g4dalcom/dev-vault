# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42895)

## 📝 문제

아래와 같이 5와 사칙연산만으로 12를 표현할 수 있습니다.

12 = 5 + 5 + (5 / 5) + (5 / 5)  
12 = 55 / 5 + 5 / 5  
12 = (55 + 5) / 5

5를 사용한 횟수는 각각 6,5,4 입니다. 그리고 이중 가장 작은 경우는 4입니다.  
이처럼 숫자 N과 number가 주어질 때, N과 사칙연산만 사용해서 표현 할 수 있는 방법 중 N 사용횟수의 최솟값을 return 하도록 solution 함수를 작성하세요.

##### 제한사항

- N은 1 이상 9 이하입니다.
- number는 1 이상 32,000 이하입니다.
- 수식에는 괄호와 사칙연산만 가능하며 나누기 연산에서 나머지는 무시합니다.
- 최솟값이 8보다 크면 -1을 return 합니다.

##### 입출력 예

|N|number|return|
|---|---|---|
|5|12|4|
|2|11|3|

##### 입출력 예 설명

예제 #1  
문제에 나온 예와 같습니다.

예제 #2  
`11 = 22 / 2`와 같이 2를 3번만 사용하여 표현할 수 있습니다.

---

### 💡 풀이


### ❌ 오답

```java
import java.util.*;

class Solution {
    static String[] operator = {"+", "-", "*", "/"};
    static int N, number;
    static int min = -1;
    
    public int solution(int N, int number) {
        this.N = N;
        this.number = number;
        
        for (int i = 1; i < 8; i++) {
            calculate(N, i, 1);
        }
     
        return min;
    }
    
    public void calculate(int value, int count, int depth) {
        if (min > 0) return;
        
        if (count == depth) {
            if (value == number) min = count;
            return;
        }
        
        for (int i = 0; i < operator.length; i++) {
            calculate(concatNumber(depth), count, depth+1);
            calculate(operation(value, operator[i]), count, depth+1);
        }
    }
    
    public int concatNumber(int depth) {
        int value = N;
        for (int i = 0; i < depth; i++) {
            value = value * 10 + N;
        }
        return value;
    }
    
    public int operation(int value, String operator) {
        switch (operator) {
            case "+": value += N;
                break;
            case "-": value -= N;
                break;
            case "*": value *= N;
                break;
            case "/": value /= N;
                break;
        }       
        return value;
    }
}
```

### 🔍 정답
