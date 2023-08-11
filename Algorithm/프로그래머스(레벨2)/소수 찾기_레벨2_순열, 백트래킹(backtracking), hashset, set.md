# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42839)

## 📝 문제

한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.

각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

- numbers는 길이 1 이상 7 이하인 문자열입니다.
- numbers는 0~9까지 숫자만으로 이루어져 있습니다.
- "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

##### 입출력 예

|numbers|return|
|---|---|
|"17"|3|
|"011"|2|

##### 입출력 예 설명

예제 #1  
[1, 7]으로는 소수 [7, 17, 71]를 만들 수 있습니다.

예제 #2  
[0, 1, 1]으로는 소수 [11, 101]를 만들 수 있습니다.

- 11과 011은 같은 숫자로 취급합니다.

---

### 💡 풀이

- \[1, 7\] 인 경우 17과 71을 모두 인정하므로(순서가 있음) 순열로 풀어야겠다고 생각하였습니다.
- 그리고 \[0, 1, 1\] 의 경우는 101과 같은 수가 중복으로 나올 수 있기 때문에 중복을 배제하는 자료구조인 Set을 이용하였습니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    
    Set<Integer> map = new HashSet<>();
    boolean[] visit;
    
    public int solution(String numbers) {
        visit = new boolean[numbers.length()];
        
        backtracking(numbers, "", 0);
        return map.size();
    }
    
    public void backtracking(String numbers, String current, int depth) {
        if (depth == numbers.length()) return;
        
        for (int i = 0; i < numbers.length(); i++) {
            if (!visit[i]) {
                visit[i] = true;
                String number = current + numbers.charAt(i);

                if (check(number)) {
                    map.add(Integer.parseInt(number));
                }
                
                backtracking(numbers, number, depth+1);
                visit[i] = false;
            }
        }
    }
    
    public boolean check(String number) {
        int num = Integer.parseInt(number);

        if (num <= 1) return false;
        
        for (int i = 2; i < num; i++) {
            if (num % i == 0) return false;
        }
        
        return true;
    }
}
```