# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/154539)

## 📝 문제

정수로 이루어진 배열 `numbers`가 있습니다. 배열 의 각 원소들에 대해 자신보다 뒤에 있는 숫자 중에서 자신보다 크면서 가장 가까이 있는 수를 뒷 큰수라고 합니다.  
정수 배열 `numbers`가 매개변수로 주어질 때, 모든 원소에 대한 뒷 큰수들을 차례로 담은 배열을 return 하도록 solution 함수를 완성해주세요. 단, 뒷 큰수가 존재하지 않는 원소는 -1을 담습니다.

---

##### 제한사항

-   4 ≤ `numbers`의 길이 ≤ 1,000,000
    -   1 ≤ `numbers[i]` ≤ 1,000,000

---

##### 입출력 예

| numbers            | result        |
|:------------------ |:------------- |
| [2, 3, 3, 5]       | [3, 5, 5, -1] |
| [9, 1, 5, 3, 6, 2] | [-1, 5, 6, 6, -1, -1]              |


---

##### 입출력 예 설명

입출력 예 #1  
2의 뒷 큰수는 3입니다. 첫 번째 3의 뒷 큰수는 5입니다. 두 번째 3 또한 마찬가지입니다. 5는 뒷 큰수가 없으므로 -1입니다. 위 수들을 차례대로 배열에 담으면 [3, 5, 5, -1]이 됩니다.

입출력 예 #2  
9는 뒷 큰수가 없으므로 -1입니다. 1의 뒷 큰수는 5이며, 5와 3의 뒷 큰수는 6입니다. 6과 2는 뒷 큰수가 없으므로 -1입니다. 위 수들을 차례대로 배열에 담으면 [-1, 5, 6, 6, -1, -1]이 됩니다.

---

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int[] solution(int[] numbers) {
        Stack<Integer> answer = new Stack<>();
        Stack<Integer> compare = new Stack<>();
        
        for (int i = numbers.length-1; i >= 0; i--) {
            while (!compare.isEmpty() && compare.peek() <= numbers[i]) {
                compare.pop();
            }
            
            if (compare.isEmpty()) {
                answer.push(-1);
            } else {
                answer.push(compare.peek());
            }
            compare.push(numbers[i]);
        }
        
        int[] result = new int[numbers.length];
        int idx = 0;
        while (!answer.isEmpty()) {
            result[idx] = answer.pop();
            idx++;
        }
        
        return result;
    }
}
```
- [오큰수 문제링크](https://github.com/g4dalcom/dev_vault/blob/main/Algorithm/%EB%B0%B1%EC%A4%80(%EA%B3%A8%EB%93%9C4)/%EC%98%A4%ED%81%B0%EC%88%98(%EB%B0%B1%EC%A4%8017298%EB%B2%88)_%EA%B3%A8%EB%93%9C4_stack.md) 에서 풀었던 방법으로 다시 풀어보았다.
- 입력받은 배열의 뒤에서부터 탐색을 하며, 
- 현재 값보다 앞 인덱스의 값이 더 크다면 현재 값은 필요 없는 수가 되므로 버리고 
- **뒤에 있는 큰 수**의 조건에 맞다면 answer에 넣어주는 방식이다.


### 💡 정답 (다른 풀이)

```java
import java.io.IOException;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        int[] answer = new int[numbers.length];
        Stack<Integer> stack = new Stack<>();

        for (int i = 0; i < numbers.length; i++) {
            while (!stack.isEmpty() && numbers[stack.peek()] < numbers[i]) {
                answer[stack.pop()] = numbers[i];
            }
            stack.add(i);
        }

        while (!stack.isEmpty()) {
            int index = stack.pop();
            answer[index] = -1;
        }
	    return answer;
    }
}
```
- stack에 인덱스를 저장해서 뒤에 큰 수를 찾으면 해당 인덱스를 탐색한 큰 수로 바꿔주는 방식이다. 
- 그리고 탐색이 끝난 뒤에도 stack에 남아있는 인덱스들은 뒤에 큰 수를 못찾은 수들이므로 해당 인덱스를 -1로 채워주는 것!