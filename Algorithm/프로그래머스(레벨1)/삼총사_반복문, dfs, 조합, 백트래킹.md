# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/131705)

## 📝 문제

한국중학교에 다니는 학생들은 각자 정수 번호를 갖고 있습니다. 이 학교 학생 3명의 정수 번호를 더했을 때 0이 되면 3명의 학생은 삼총사라고 합니다. 예를 들어, 5명의 학생이 있고, 각각의 정수 번호가 순서대로 -2, 3, 0, 2, -5일 때, 첫 번째, 세 번째, 네 번째 학생의 정수 번호를 더하면 0이므로 세 학생은 삼총사입니다. 또한, 두 번째, 네 번째, 다섯 번째 학생의 정수 번호를 더해도 0이므로 세 학생도 삼총사입니다. 따라서 이 경우 한국중학교에서는 두 가지 방법으로 삼총사를 만들 수 있습니다.

한국중학교 학생들의 번호를 나타내는 정수 배열 `number`가 매개변수로 주어질 때, 학생들 중 삼총사를 만들 수 있는 방법의 수를 return 하도록 solution 함수를 완성하세요.

---

##### 제한사항

-   3 ≤ `number`의 길이 ≤ 13
-   -1,000 ≤ `number`의 각 원소 ≤ 1,000
-   서로 다른 학생의 정수 번호가 같을 수 있습니다.

---

##### 입출력 예

| number                   | result |
|:------------------------ |:------ |
| [-2, 3, 0, 2, -5]        | 2      |
| [-3, -2, -1, 0, 1, 2, 3] | 5      |
| [-1, 1, -1, 1]           | 0       |


---

##### 입출력 예 설명

**입출력 예 #1**

-   문제 예시와 같습니다.

**입출력 예 #2**

-   학생들의 정수 번호 쌍 (-3, 0, 3), (-2, 0, 2), (-1, 0, 1), (-2, -1, 3), (-3, 1, 2) 이 삼총사가 될 수 있으므로, 5를 return 합니다.

**입출력 예 #3**

-   삼총사가 될 수 있는 방법이 없습니다.

---

### 🔍 정답

```java
import java.util.*;

class Solution {
    
    static boolean[] visit;
    static int[] result;
    static int answer = 0;
    
    public int solution(int[] number) {
        
        result = new int[number.length];
        visit = new boolean[number.length];
        
        dfs(0, number, 0);
        
        return answer;
    }
    
    public static void dfs(int num, int[] number, int depth) {
        
        if (depth == 3) {
            int sum = 0;
            for (int i = 0; i < depth; i ++) {
                sum += result[i];
            }
            
            if (sum == 0) {
                answer++;
            }
            return;
        }

        for (int i = num; i < number.length; i++) {
            if (!visit[i]) {
                visit[i] = true;
                result[depth] = number[i];
                dfs(i, number, depth+1);
                visit[i] = false;
            }
        }
    }
}
```
- 조합 문제라고 생각해서 dfs를 이용하였다. 이후에 다른 사람들 풀이를 보니 훨씬 간단하고 직관적으로 해결하여서 많이 보고 배웠다!


### 💡 다른 풀이

```java
class Solution {
    public int solution(int[] number) {
        int answer = 0;
        for (int i = 0; i < number.length; i++) {
            for (int j = i + 1; j < number.length; j++) {
                for (int k = j + 1; k < number.length; k++) {
                    if (number[i] + number[j] + number[k] == 0) {
                        answer++;
                    }
                }
            }
        }
        return answer;
    }
}
```
- 직관적인 반복문 풀이!

### 💡 다른 풀이 

```java
import java.util.*;
import java.io.*;

class Solution {
    static int answer, N;
    static int[] selected;

    public int solution(int[] number) {
        N = 3;
        comb(0, 0, 0, number);
        return answer;
    }

    public static void comb(int cur, int cnt, int sum, int[] number) {
        if (cnt == N) {
            if (sum == 0)
                answer++;
            return;
        }

        for (int i = cur; i < number.length; i++) {
            comb(i + 1, cnt + 1, sum + number[i], number);
        }
    }
}
```
- 나와 유사하게 백트래킹 방법으로 풀었지만 훨씬 간결해보여서 감탄했다....