# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/214288)

## 📝 문제

현대모비스는 우수한 SW 인재 채용을 위해 상시로 채용 설명회를 진행하고 있습니다. 채용 설명회에서는 채용과 관련된 상담을 원하는 참가자에게 멘토와 1:1로 상담할 수 있는 기회를 제공합니다. 채용 설명회에는 멘토 `n`명이 있으며, 1~`k`번으로 분류되는 상담 유형이 있습니다. 각 멘토는 `k`개의 상담 유형 중 하나만 담당할 수 있습니다. 멘토는 자신이 담당하는 유형의 상담만 가능하며, 다른 유형의 상담은 불가능합니다. 멘토는 동시에 참가자 한 명과만 상담 가능하며, 상담 시간은 정확히 참가자가 요청한 시간만큼 걸립니다.

참가자가 상담 요청을 하면 아래와 같은 규칙대로 상담을 진행합니다.

- 상담을 원하는 참가자가 상담 요청을 했을 때, 참가자의 상담 유형을 담당하는 멘토 중 상담 중이 아닌 멘토와 상담을 시작합니다.
- 만약 참가자의 상담 유형을 담당하는 멘토가 모두 상담 중이라면, 자신의 차례가 올 때까지 기다립니다. **참가자가 기다린 시간은 참가자가 상담 요청했을 때부터 멘토와 상담을 시작할 때까지의 시간입니다.**
- 모든 멘토는 상담이 끝났을 때 자신의 상담 유형의 상담을 받기 위해 기다리고 있는 참가자가 있으면 즉시 상담을 시작합니다. 이때, **먼저 상담 요청한 참가자가 우선됩니다.**

참가자의 상담 요청 정보가 주어질 때, 참가자가 상담을 요청했을 때부터 상담을 시작하기까지 기다린 시간의 합이 최소가 되도록 각 상담 유형별로 멘토 인원을 정하려 합니다. **단, 각 유형별로 멘토 인원이 적어도 한 명 이상이어야 합니다.**

예를 들어, 5명의 멘토가 있고 1~3번의 3가지 상담 유형이 있을 때 아래와 같은 참가자의 상담 요청이 있습니다.

**참가자의 상담 요청**

|참가자 번호|시각|상담 시간|상담 유형|
|---|---|---|---|
|1번 참가자|10분|60분|1번 유형|
|2번 참가자|15분|100분|3번 유형|
|3번 참가자|20분|30분|1번 유형|
|4번 참가자|30분|50분|3번 유형|
|5번 참가자|50분|40분|1번 유형|
|6번 참가자|60분|30분|2번 유형|
|7번 참가자|65분|30분|1번 유형|
|8번 참가자|70분|100분|2번 유형|

이때, 멘토 인원을 아래와 같이 정하면, 참가자가 기다린 시간의 합이 25로 최소가 됩니다.

|1번 유형|2번 유형|3번 유형|
|---|---|---|
|2명|1명|2명|

**1번 유형**

1번 유형을 담당하는 멘토가 2명 있습니다.

- 1번 참가자가 상담 요청했을 때, 멘토#1과 10분~70분 동안 상담을 합니다.
- 3번 참가자가 상담 요청했을 때, 멘토#2와 20분~50분 동안 상담을 합니다.
- 5번 참가자가 상담 요청했을 때, 멘토#2와 50분~90분 동안 상담을 합니다.
- 7번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 1번 참가자의 상담이 끝날 때까지 5분 동안 기다리고 멘토#1과 70분~100분 동안 상담을 합니다.

**2번 유형**

2번 유형을 담당하는 멘토가 1명 있습니다.

- 6번 참가자가 상담 요청했을 때, 멘토와 60분~90분 동안 상담을 합니다.
- 8번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 6번 참가자의 상담이 끝날 때까지 20분 동안 기다리고 90분~190분 동안 상담을 합니다.

**3번 유형**

3번 유형을 담당하는 멘토가 2명 있습니다.

- 2번 참가자가 상담 요청했을 때, 멘토#1과 15분~115분 동안 상담을 합니다.
- 4번 참가자가 상담 요청했을 때, 멘토#2와 30분~80분 동안 상담을 합니다.

상담 유형의 수를 나타내는 정수 `k`, 멘토의 수를 나타내는 정수 `n`과 참가자의 상담 요청을 담은 2차원 정수 배열 `reqs`가 매개변수로 주어집니다. 멘토 인원을 적절히 배정했을 때 참가자들이 상담을 받기까지 기다린 시간을 모두 합한 값의 최솟값을 return 하도록 solution 함수를 완성해 주세요.

---

##### 제한사항

- 1 ≤ `k` ≤ 5
- `k` ≤ `n` ≤ 20
- 3 ≤ `reqs`의 길이 ≤ 300
    - `reqs`의 원소는 [`a`, `b`, `c`] 형태의 길이가 3인 정수 배열이며, `c`번 유형의 상담을 원하는 참가자가 `a`분에 `b`분 동안의 상담을 요청했음을 의미합니다.
    - 1 ≤ `a` ≤ 1,000
    - 1 ≤ `b` ≤ 100
    - 1 ≤ `c` ≤ `k`
    - `reqs`는 `a`를 기준으로 오름차순으로 정렬되어 있습니다.
    - `reqs` 배열에서 `a`는 중복되지 않습니다. 즉, 참가자가 상담 요청한 시각은 모두 다릅니다.

---

##### 입출력 예

|k|n|reqs|result|
|---|---|---|---|
|3|5|[[10, 60, 1], [15, 100, 3], [20, 30, 1], [30, 50, 3], [50, 40, 1], [60, 30, 2], [65, 30, 1], [70, 100, 2]]|25|
|2|3|[[5, 55, 2], [10, 90, 2], [20, 40, 2], [50, 45, 2], [100, 50, 2]]|90|

---

##### 입출력 예 설명

**입출력 예 #1**

문제 예시와 같습니다.

**입출력 예 #2**

**참가자의 상담 요청**

|참가자 번호|시각|상담 시간|상담 유형|
|---|---|---|---|
|1번 참가자|5분|55분|2번 유형|
|2번 참가자|10분|90분|2번 유형|
|3번 참가자|20분|40분|2번 유형|
|4번 참가자|50분|45분|2번 유형|
|5번 참가자|100분|50분|2번 유형|

멘토 인원을 아래와 같이 정하면, 참가자가 기다린 시간의 합이 90으로 최소가 됩니다.

|1번 유형|2번 유형|
|---|---|
|1명|2명|

**1번 유형**

1번 유형을 담당하는 멘토가 1명 있습니다. 1번 유형의 상담을 요청한 참가자가 없지만, 유형별로 멘토 인원이 적어도 한 명 이상이어야 합니다.

**2번 유형**

2번 유형을 담당하는 멘토가 2명 있습니다.

- 1번 참가자가 상담 요청했을 때, 멘토#1과 5분~60분 동안 상담을 합니다.
- 2번 참가자가 상담 요청했을 때, 멘토#2와 10분~100분 동안 상담을 합니다.
- 3번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 1번 참가자의 상담이 끝날 때까지 40분 동안 기다리고 멘토#1과 60분~100분 동안 상담을 합니다. 1번 참가자의 상담이 끝났을 때 4번 참가자도 기다리고 있었지만, 먼저 상담 요청한 3번 참가자가 우선됩니다.
- 4번 참가자가 상담 요청했을 때, 모든 멘토가 상담 중이므로 2번 참가자의 상담이 끝날 때까지 50분 동안 기다리고 멘토#2와 100분~145분 동안 상담을 합니다. 이때, 멘토#1과 상담할 수도 있지만 어느 멘토와 상담해도 상관없습니다.
- 5번 참가자가 상담 요청했을 때, 멘토#1과 100분~150분 동안 상담을 합니다.

따라서 90을 return 하면 됩니다.

---

### 💡 풀이

- 몇 번의 시행착오 끝에 풀 수 있었습니다 ㅠㅠ
- 첫 번째로는 각 유형마다 참가자에 따른 대기시간을 모두 계산하였습니다. 1번 예제라면, 1번 유형은 50분, 50분, 85분이고 2번 유형은 20분인데 여기서 기다리는 시간이 큰 순서대로 해당 유형에 인원을 배치하는 것이었습니다.
- 그러나 이 방법은 몇몇개의 테스트 케이스만 통과되어서 접근방법이 잘못되었다고 판단하였습니다.
- 그리고 두 번째로는 완전 탐색(DFS)으로 풀어보았는데요. mentor 배열의 인덱스를 각각의 유형으로 놓고 모두 1로 초기화를 한 뒤(최소 1명의 멘토가 배치되므로) n-k명을 이리저리 분배해보며 최솟값을 찾는 것이었죠!

```java
public void dfs(int k, int n, int depth) {
	if (depth == n) {
		int sum = 0;
		for (int i = 1; i <= k; i++) {
			sum += counseling(i, mentor[i]);
		}
		waitingTime = Math.min(waitingTime, sum);
		return;
	}
	
	for (int i = 1; i <= k; i++) {
		mentor[i]++;
		dfs(k, n, depth+1);
		mentor[i]--;
	}
}
```

- 이 방법은 초반 테스트케이스는 통과하지만 후반부 테스트케이스는 시간 초과가 나게 됩니다 ㅠㅠ
- 마지막으로는 모든 유형에 대해서 1명부터 최대로 배치할 수 있는 인원인 n - k + 1명까지 배치했을 때의 소요 시간을 2차원 배열에 저장해두고 1명일 때와 2명일 때를 비교해서 인원이 늘었을 때 가장 많은 시간이 줄어드는 유형에 인원을 배치하는 방식으로 접근하였습니다.
- 2차원 배열에 모든 경우의 수를 저장해두기 때문에 이후에 멘토가 모두 효율적으로 배치된 후에는 저장된 값만으로 계산할 수도 있었습니다!


### 🔍 정답

```java
import java.util.*;

class Solution {
    static ArrayList<ArrayList<Counsel>> list = new ArrayList<>();
    static int[] mentor;
    static int[][] time;

    public int solution(int k, int n, int[][] reqs) {
        mentor = new int[k+1];
        time = new int[k+1][n+1];
        Arrays.fill(mentor, 1);

        for (int i = 0; i < k+1; i++) {
            list.add(new ArrayList<>());
        }

        for (int[] req : reqs) {
            int startTime = req[0];
            int duration = req[1];
            int type = req[2];

            list.get(type).add(new Counsel(startTime, duration));
        }

        calculateTime(k, n);
        placeMentor(k, n);

        int waitingTime = 0;
        for (int i = 1; i < mentor.length; i++) {
            waitingTime += time[i][mentor[i]];
        }

        return waitingTime;
    }

    public void calculateTime(int k, int n) {
        for (int i = 1; i <= k; i++) {
            for (int j = 1; j <= n-k+1; j++) {
                time[i][j] = counseling(i, j);
            }
        }
    }

    public int counseling(int type, int number) {
        int time = 0;
        PriorityQueue<Integer> pq = new PriorityQueue<>();

        for (Counsel c : list.get(type)) {
            if (pq.size() < number) {
                pq.add(c.startTime + c.duration);
            } else {
                int prev = pq.poll();
                if (prev > c.startTime) {
                    time += prev - c.startTime;
                    pq.add(prev + c.duration);
                } else {
                    pq.add(c.startTime + c.duration);
                }
            }
        }

        return time;
    }

    public void placeMentor(int k, int n) {        
        int loop = n - k;

        while (loop-- > 0) {
            int maxTime = 0;
            int index = 0;
            for (int i = 1; i <= k; i++) {
                int diff = time[i][mentor[i]] - time[i][mentor[i] + 1];

                if (maxTime < diff) {
                    maxTime = diff;
                    index = i;
                }
            }
            mentor[index]++;
        }
    }

    public class Counsel {
        int startTime;
        int duration;

        public Counsel(int startTime, int duration) {
            this.startTime = startTime;
            this.duration = duration;
        }
    }
}
```


### ❌ 오답

```java
import java.util.*;

class Solution {
    static ArrayList<ArrayList<Counsel>> list = new ArrayList<>();
    static int waitingTime = 0;
    static PriorityQueue<Waiting> waitQ = new PriorityQueue<>();
    
    public int solution(int k, int n, int[][] reqs) {
        int[] mentor = new int[k+1];
        
        for (int i = 0; i < k+1; i++) {
            list.add(new ArrayList<>());
        }
        
        for (int[] req : reqs) {
            int startTime = req[0];
            int duration = req[1];
            int type = req[2];
            
            list.get(type).add(new Counsel(startTime, duration));
        }
        
        for (int i = 1; i < list.size(); i++) {
            timeCalculate(i);
        }
        
        int loop = n-k;
        while (loop-- > 0) {
            Waiting w = waitQ.poll();
            mentor[w.type]++;
        }
        
        for (int i = 1; i <= k; i++) {
            counseling(i, mentor);
        }
        
        return waitingTime;
    }
    
    public void timeCalculate(int type) {
        int time = 0;
        int prev = 0;
        for (Counsel c : list.get(type)) {
            if (prev > c.startTime) {
                time += prev - c.startTime;
            } else {
                prev = c.startTime + c.duration;
            }
        }
        waitQ.add(new Waiting(type, time));
    }
    
    public void counseling(int type, int[] mentor) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        int counselor = mentor[type] + 1;
        
        for (Counsel c : list.get(type)) {
            if (pq.size() < counselor) {
                pq.add(c.startTime + c.duration);
            } else {
                int prev = pq.poll();
                if (prev > c.startTime) {
                    waitingTime += prev - c.startTime;
                }
                pq.add(prev + c.duration);
            }
        }
    }
    
    public class Counsel {
        int startTime;
        int duration;
        
        public Counsel(int startTime, int duration) {
            this.startTime = startTime;
            this.duration = duration;
        }
    }
    
    public class Waiting implements Comparable<Waiting> {
        int type;
        int waitingTime;
        
        public Waiting(int type, int waitingTime) {
            this.type = type;
            this.waitingTime = waitingTime;
        }
        
        @Override
        public int compareTo(Waiting w) {
            return w.waitingTime - this.waitingTime;
        }
    }
}
```


### ❌ 오답

```java
import java.util.*;

class Solution {
    static ArrayList<ArrayList<Counsel>> list = new ArrayList<>();
    static int[] mentor;
    static int waitingTime = Integer.MAX_VALUE;
    
    public int solution(int k, int n, int[][] reqs) {
        mentor = new int[k+1];
        Arrays.fill(mentor, 1);
        
        for (int i = 0; i < k+1; i++) {
            list.add(new ArrayList<>());
        }
        
        for (int[] req : reqs) {
            int startTime = req[0];
            int duration = req[1];
            int type = req[2];
            
            list.get(type).add(new Counsel(startTime, duration));
        }
        
        dfs(k, n, k);
        
        return waitingTime;
    }
    
    public void dfs(int k, int n, int depth) {
        if (depth == n) {
            int sum = 0;
            for (int i = 1; i <= k; i++) {
                sum += counseling(i, mentor[i]);
            }
            waitingTime = Math.min(waitingTime, sum);
            return;
        }
        
        for (int i = 1; i <= k; i++) {
            mentor[i]++;
            dfs(k, n, depth+1);
            mentor[i]--;
        }
    }
    
    public int counseling(int type, int number) {
        int time = 0;
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        
        for (Counsel c : list.get(type)) {
            if (pq.size() < number) {
                pq.add(c.startTime + c.duration);
            } else {
                int prev = pq.poll();
                if (prev > c.startTime) {
                    time += prev - c.startTime;
                    pq.add(prev + c.duration);
                } else {
                    pq.add(c.startTime + c.duration);
                }
            }
        }

        return time;
    }
    
    public class Counsel {
        int startTime;
        int duration;
        
        public Counsel(int startTime, int duration) {
            this.startTime = startTime;
            this.duration = duration;
        }
    }
}
```