# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12987)

## 📝 문제

xx 회사의 2xN명의 사원들은 N명씩 두 팀으로 나눠 숫자 게임을 하려고 합니다. 두 개의 팀을 각각 A팀과 B팀이라고 하겠습니다. 숫자 게임의 규칙은 다음과 같습니다.

- 먼저 모든 사원이 무작위로 자연수를 하나씩 부여받습니다.
- 각 사원은 딱 한 번씩 경기를 합니다.
- 각 경기당 A팀에서 한 사원이, B팀에서 한 사원이 나와 서로의 수를 공개합니다. 그때 숫자가 큰 쪽이 승리하게 되고, 승리한 사원이 속한 팀은 승점을 1점 얻게 됩니다.
- 만약 숫자가 같다면 누구도 승점을 얻지 않습니다.

전체 사원들은 우선 무작위로 자연수를 하나씩 부여받았습니다. 그다음 A팀은 빠르게 출전순서를 정했고 자신들의 출전 순서를 B팀에게 공개해버렸습니다. B팀은 그것을 보고 자신들의 최종 승점을 가장 높이는 방법으로 팀원들의 출전 순서를 정했습니다. 이때의 B팀이 얻는 승점을 구해주세요.  
A 팀원들이 부여받은 수가 출전 순서대로 나열되어있는 배열 `A`와 i번째 원소가 B팀의 i번 팀원이 부여받은 수를 의미하는 배열 `B`가 주어질 때, B 팀원들이 얻을 수 있는 최대 승점을 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

- `A`와 `B`의 길이는 같습니다.
- `A`와 `B`의 길이는 `1` 이상 `100,000` 이하입니다.
- `A`와 `B`의 각 원소는 `1` 이상 `1,000,000,000` 이하의 자연수입니다.

---

##### 입출력 예

|A|B|result|
|---|---|---|
|[5,1,3,7]|[2,2,6,8]|3|
|[2,2,2,2]|[1,1,1,1]|0|

##### 입출력 예 설명

입출력 예 #1  
![number_game2_yt913p.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/0de59edf-76e1-4313-984a-4b2bd40911fb/number_game2_yt913p.png)  
A 팀은 숫자 5를 부여받은 팀원이 첫번째로 출전하고, 이어서 1,3,7을 부여받은 팀원들이 차례대로 출전합니다.  
B 팀원들을 4번, 2번, 3번, 1번의 순서대로 출전시킬 경우 팀원들이 부여받은 숫자들은 차례대로 8,2,6,2가 됩니다. 그러면, 첫 번째, 두 번째, 세 번째 경기에서 승리하여 3점을 얻게 되고, 이때가 최대의 승점입니다.

입출력 예 #2  
B 팀원들을 어떤 순서로 출전시켜도 B팀의 승점은 0점입니다.


---

### 💡 풀이

- 첫 번째는 모든 경우의 수를 계산하는 백트래킹 방식으로 풀이해보았는데 역시나 시간초과가 발생했습니다.
- 두 번째 방법으로는 백트래킹 로직을 조금 개선해보았는데요.
- 만약 지금까지 계산한 것 중 가장 패배가 적은 게 1패라면, 다른 경우의 수를 계산하는 도중 2패가 되면 추가적으로 더 계산할 필요가 없어집니다. 따라서 모든 경우의 수를 탐색하는 것보다는 효율적이라고 생각했습니다. 그러나 이 방법도 마찬가지로 시간초과가 발생합니다.
- 그래서 완전 탐색으로는 풀이할 수 없다고 생각했고 새로운 아이디어를 고민해보았습니다.
- A = \[5, 1, 3, 7\], B = \[2, 2, 6, 8\] 인데 우리는 B의 순서를 가지고 놀 수 있습니다. 그런데 이말은 곧 A의 순서를 섞어도 상관없다는 말입니다. 우리는 A의 각 선수 번호에 대한 B의 선수 번호를 알아야 하는 게 아니기 때문입니다.
- 그러면 우리는 정렬을 사용할 수가 있게 됩니다.
- A = \[1, 3, 5, 7\], B = \[2, 2, 6, 8\] 로 오름차순 정렬을 한 뒤 비교를 하는데 만약 B의 제일 작은 값이 A의 제일 작은 값을 이기지 못한다면 그 B는 어떤 선수와 붙어도 승점을 따내지 못하는 선수입니다.
- 그러면 B의 다음 선수와 A의 제일 작은 값을 다시 비교하는 것이죠.
- 이러한 로직이라면 단 한 번의 탐색으로 최선의 경우의 수를 구할 수 있습니다!
- 같은 로직으로 메모리는 좀 더 사용하지만 우선순위큐를 사용해서 구해도 통과가 됩니다!

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(int[] A, int[] B) {
        Arrays.sort(A);
        Arrays.sort(B);
        
        int score = 0;
        int checkIndex = 0;
        while (checkIndex < B.length) {
            for (int a : A) {
                while (checkIndex < B.length && B[checkIndex] <= a) checkIndex++;
                if (checkIndex < B.length && B[checkIndex] > a) {
                    score++;
                    checkIndex++;
                }
            }
        }
        
        return score;
    }
}
```


### 🔍 정답(우선순위 큐)

```java
import java.util.*;

class Solution {
    public int solution(int[] A, int[] B) {
        int score = 0;
        PriorityQueue<Integer> teamA = new PriorityQueue<>();
        PriorityQueue<Integer> teamB = new PriorityQueue<>();
        
        for (int i = 0; i < A.length; i++) {
            teamA.offer(A[i]);
            teamB.offer(B[i]);
        }
        
        while (!teamA.isEmpty() && !teamB.isEmpty()) {
            int playerA = teamA.peek();
            int playerB = teamB.peek();
            
            if (playerA >= playerB) {
                teamB.poll();
            } else {
                teamA.poll();
                teamB.poll();
                score++;
            }
        }
        
        return score;
    }
}
```



### ❌ 시간 초과(백트래킹)

```java
class Solution {
    static int maxScore = 0;
    static boolean[] visit;
    
    public int solution(int[] A, int[] B) {
        int[] combinationB = new int[B.length];
        visit = new boolean[B.length];
        
        backtracking(A, B, combinationB, 0);
        
        return maxScore;
    }
    
    public void backtracking(int[] A, int[] B, int[] combination, int depth) {
        if (depth == A.length) {
            calculate(A, combination);
            return;
        }
        
        for (int i = 0; i < combination.length; i++) {
            if (!visit[i]) {
                combination[depth] = B[i];
                visit[i] = true;
                backtracking(A, B, combination, depth+1);
                visit[i] = false;
            }
        }
    }
    
    public void calculate(int[] A, int[] combination) {
        int score = 0;
        
        for (int i = 0; i < A.length; i++) {
            if (A[i] < combination[i]) score++;
        }
        
        maxScore = Math.max(maxScore, score);
    }
}
```


### ❌ 시간초과(개선된 백트래킹)

```java
class Solution {
    static int minLoseCount;
    static boolean[] visit;
    
    public int solution(int[] A, int[] B) {
        int[] combinationB = new int[B.length];
        visit = new boolean[B.length];
        minLoseCount = B.length;

        backtracking(A, B, combinationB, 0, 0);
        
        return B.length - minLoseCount;
    }
    
    public void backtracking(int[] A, int[] B, int[] combination, int depth, int loseCount) {    
        if (minLoseCount < loseCount) return;
        
        if (depth == A.length) {
            minLoseCount = loseCount;
            return;
        }
        
        for (int i = 0; i < combination.length; i++) {
            if (!visit[i]) {
                combination[depth] = B[i];
                visit[i] = true;
                if (A[depth] > combination[depth]) {
                    backtracking(A, B, combination, depth+1, loseCount+1);
                } else {
                    backtracking(A, B, combination, depth+1, loseCount);
                }
                visit[i] = false;
            }
        }
    }
}
```