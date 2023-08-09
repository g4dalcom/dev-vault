# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42584)

## 📝 문제

초 단위로 기록된 주식가격이 담긴 배열 prices가 매개변수로 주어질 때, 가격이 떨어지지 않은 기간은 몇 초인지를 return 하도록 solution 함수를 완성하세요.

##### 제한사항

- prices의 각 가격은 1 이상 10,000 이하인 자연수입니다.
- prices의 길이는 2 이상 100,000 이하입니다.

##### 입출력 예

|prices|return|
|---|---|
|[1, 2, 3, 2, 3]|[4, 3, 1, 1, 0]|

##### 입출력 예 설명

- 1초 시점의 ₩1은 끝까지 가격이 떨어지지 않았습니다.
- 2초 시점의 ₩2은 끝까지 가격이 떨어지지 않았습니다.
- 3초 시점의 ₩3은 1초뒤에 가격이 떨어집니다. 따라서 1초간 가격이 떨어지지 않은 것으로 봅니다.
- 4초 시점의 ₩2은 1초간 가격이 떨어지지 않았습니다.
- 5초 시점의 ₩3은 0초간 가격이 떨어지지 않았습니다.

---

### 💡 풀이

- 문제 유형이 스택이기도 했고 이전에 풀었던 스택 문제와 비슷한 문제라 스택을 이용해서 풀어보려고 했습니다.
- 기본 골자는, prices 배열을 순회하면서 이전 값보다 작은 수가 나오면(가격이 떨어진 경우) **현재 인덱스 - 이전 인덱스**를 해주는 것입니다. 이 때 그 이전의 값들도 반복해서 확인하게 됩니다.
	- /[1 3 4 2 3/] 의 경우 index 2와 3을 비교할 때 위 조건에 걸리므로 3-2=1을 해주고 인덱스 1번인 3 또한 3번 인덱스보다 크므로 3-1=2까지 해주는 식입니다.
- 그리고 스택이 비어있지 않다면 해당 인덱스의 수들은 마지막까지 계속해서 수가 줄어들지 않은 경우이므로 **prices.length - 인덱스 - 1** 을 해주었습니다.


### 🔍 정답

```java
import java.util.*;

class Solution {
    public int[] solution(int[] prices) {
        int[] answer = new int[prices.length];
        Stack<Integer> stack = new Stack<>();
        
        for (int i = 0; i < prices.length; i++) {
            while (!stack.isEmpty() && prices[i] < prices[stack.peek()]) {
                answer[stack.peek()] = i - stack.peek();
                stack.pop();
            }
            stack.push(i);
        }
        
        while (!stack.isEmpty()) {
            answer[stack.peek()] = prices.length - stack.peek() - 1;
            stack.pop();
        }
        
        return answer;
    }
}
```

### 🔍 다른 풀이(반복문)

```java
class Solution {
    public int[] solution(int[] prices) {
        int len = prices.length;
        int[] answer = new int[len];
        int i, j;
        for (i = 0; i < len; i++) {
            for (j = i + 1; j < len; j++) {
                answer[i]++;
                if (prices[i] > prices[j])
                    break;
            }
        }
        return answer;
    }
}
```

- prices의 길이가 최대 10,000이기 때문에 반복문으로도 통과가 되는 것 같네요!
- 문제를 처음 접했을 때 바로 생각할 수 있는 풀이법이고 코드가 직관적이라는 점이 좋은 것 같습니다.

### 🔍 다른 풀이(클래스)

```java
import java.util.Stack;

class Solution {
    public int[] solution(int[] prices) {
        int[] answer = new int[prices.length];

        Stack<Stock> stack = new Stack<>();

        stack.push(new Stock(0, prices[0]));
        for (int i = 1; i < prices.length ; i++) {
            int price = prices[i];
            while((!stack.isEmpty()) && (stack.peek().getCost() > price)) {
                Stock item = stack.pop();
                answer[item.getIndex()] = i - item.index;
            }

            stack.push(new Stock(i, prices[i]));
        }

        int lastIndex = stack.pop().index;
        answer[lastIndex] = 0;

        // stack 비우기
        while(!stack.isEmpty()) {
            Stock item = stack.pop();
            answer[item.getIndex()] = lastIndex - item.index;
        }

        return answer;
    }

    public static class Stock {
        private int index;
        private int cost;

        public Stock(int index, int cost) {
            this.index = index;
            this.cost = cost;
        }

        public int getCost() {
            return cost;
        }

        public int getIndex() {
            return index;
        }
    }
}
```

- 기본적인 풀이법은 동일한데 클래스를 이용해서 요소들을 관리한 게 재밌었습니다.
- 클래스 특성상 코드가 길어지는 점은 단점이지만 좀 더 객체지향스러운 풀이인 것 같습니다!