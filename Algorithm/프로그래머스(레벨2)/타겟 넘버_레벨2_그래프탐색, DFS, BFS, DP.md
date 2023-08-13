# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/43165)

## 📝 문제

n개의 음이 아닌 정수들이 있습니다. 이 정수들을 순서를 바꾸지 않고 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다. 예를 들어 [1, 1, 1, 1, 1]로 숫자 3을 만들려면 다음 다섯 방법을 쓸 수 있습니다.

```
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3
```

사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때 숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 주어지는 숫자의 개수는 2개 이상 20개 이하입니다.
- 각 숫자는 1 이상 50 이하인 자연수입니다.
- 타겟 넘버는 1 이상 1000 이하인 자연수입니다.

##### 입출력 예

|numbers|target|return|
|---|---|---|
|[1, 1, 1, 1, 1]|3|5|
|[4, 1, 2, 1]|4|2|

##### 입출력 예 설명

**입출력 예 #1**

문제 예시와 같습니다.

**입출력 예 #2**

```
+4+1-2+1 = 4
+4-1+2-1 = 4
```

- 총 2가지 방법이 있으므로, 2를 return 합니다.

---

### 💡 풀이

- 그래프 탐색의 기본적인 문제라고 생각합니다.
- DFS는 모든 경우의 수를 재귀를 통해 계산하는 것이고 BFS는 계산값(value)과 인덱스(index)를 가지는 객체와 큐를 이용한 방법입니다. 
- BFS의 경우 새로운 객체를 계속 생성하고 큐에 I/O 작업을 계속해야 하기 때문에 성능상 재귀보다 불리한 경우가 많을 것 같습니다.

### 🔍 정답(DFS)

```java
class Solution {
    static int count;
    public int solution(int[] numbers, int target) {
        dfs(0, numbers, 0, target);
        
        return count;
    }
    
    public void dfs(int sum, int[] numbers, int depth, int target) {
        if (depth == numbers.length) {
            if (sum == target) count++;
            return;
        }
        
        dfs(sum+numbers[depth], numbers, depth+1, target);
        dfs(sum-numbers[depth], numbers, depth+1, target);
    }
}
```


### 🔍 정답(BFS)

```java
import java.util.*;

class Solution {
    static int count;
    
    public int solution(int[] numbers, int target) {
        bfs(numbers, target);
        
        return count;
    }
    
    public void bfs(int[] numbers, int target) {
        Queue<Calc> q = new LinkedList<>();
        q.add(new Calc(numbers[0], 0));
        q.add(new Calc(-numbers[0], 0));
        
        while (!q.isEmpty()) {
            Calc cur = q.poll();
            
            if (cur.index == numbers.length-1) {
                if (cur.value == target) count++;
                continue;
            }
            
            int plus = cur.value + numbers[cur.index+1];
            int minus = cur.value - numbers[cur.index+1];
            
            q.add(new Calc(plus, cur.index+1));
            q.add(new Calc(minus, cur.index+1));
        }
    }
    
    class Calc {
        int value;
        int index;
        
        public Calc(int value, int index) {
            this.value = value;
            this.index = index;
        }
    }
}
```


### 🔍 정답(DP)

```java
class Solution {
    public int solution(int[] numbers, int target) {
        return dp(0, numbers, 0, target);
    }
    
    public int dp(int sum, int[] numbers, int depth, int target) {
        if (depth == numbers.length) {
            if (sum == target) return 1;
            return 0;
        }
        
        return dp(sum+numbers[depth], numbers, depth+1, target) + dp(sum-numbers[depth], numbers, depth+1, target);
    }
}
```

- 좋아요를 가장 많이 받은 풀이입니다.
- DFS 풀이와 유사한데 직접적인 값(1, 0)을 리턴하고 그 값을 누적해서 계산하는 방법입니다!