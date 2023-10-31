# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42627)

## 📝 문제

하드디스크는 한 번에 하나의 작업만 수행할 수 있습니다. 디스크 컨트롤러를 구현하는 방법은 여러 가지가 있습니다. 가장 일반적인 방법은 요청이 들어온 순서대로 처리하는 것입니다.

예를들어

```
- 0ms 시점에 3ms가 소요되는 A작업 요청
- 1ms 시점에 9ms가 소요되는 B작업 요청
- 2ms 시점에 6ms가 소요되는 C작업 요청
```

와 같은 요청이 들어왔습니다. 이를 그림으로 표현하면 아래와 같습니다.  
![Screen Shot 2018-09-13 at 6.34.58 PM.png](https://grepp-programmers.s3.amazonaws.com/files/production/b68eb5cec6/38dc6a53-2d21-4c72-90ac-f059729c51d5.png)

한 번에 하나의 요청만을 수행할 수 있기 때문에 각각의 작업을 요청받은 순서대로 처리하면 다음과 같이 처리 됩니다.  
![Screen Shot 2018-09-13 at 6.38.52 PM.png](https://grepp-programmers.s3.amazonaws.com/files/production/5e677b4646/90b91fde-cac4-42c1-98b8-8f8431c52dcf.png)

```
- A: 3ms 시점에 작업 완료 (요청에서 종료까지 : 3ms)
- B: 1ms부터 대기하다가, 3ms 시점에 작업을 시작해서 12ms 시점에 작업 완료(요청에서 종료까지 : 11ms)
- C: 2ms부터 대기하다가, 12ms 시점에 작업을 시작해서 18ms 시점에 작업 완료(요청에서 종료까지 : 16ms)
```

이 때 각 작업의 요청부터 종료까지 걸린 시간의 평균은 10ms(= (3 + 11 + 16) / 3)가 됩니다.

하지만 A → C → B 순서대로 처리하면  
![Screen Shot 2018-09-13 at 6.41.42 PM.png](https://grepp-programmers.s3.amazonaws.com/files/production/9eb7c5a6f1/a6cff04d-86bb-4b5b-98bf-6359158940ac.png)

```
- A: 3ms 시점에 작업 완료(요청에서 종료까지 : 3ms)
- C: 2ms부터 대기하다가, 3ms 시점에 작업을 시작해서 9ms 시점에 작업 완료(요청에서 종료까지 : 7ms)
- B: 1ms부터 대기하다가, 9ms 시점에 작업을 시작해서 18ms 시점에 작업 완료(요청에서 종료까지 : 17ms)
```

이렇게 A → C → B의 순서로 처리하면 각 작업의 요청부터 종료까지 걸린 시간의 평균은 9ms(= (3 + 7 + 17) / 3)가 됩니다.

각 작업에 대해 [작업이 요청되는 시점, 작업의 소요시간]을 담은 2차원 배열 jobs가 매개변수로 주어질 때, 작업의 요청부터 종료까지 걸린 시간의 평균을 가장 줄이는 방법으로 처리하면 평균이 얼마가 되는지 return 하도록 solution 함수를 작성해주세요. (단, 소수점 이하의 수는 버립니다)

##### 제한 사항

- jobs의 길이는 1 이상 500 이하입니다.
- jobs의 각 행은 하나의 작업에 대한 [작업이 요청되는 시점, 작업의 소요시간] 입니다.
- 각 작업에 대해 작업이 요청되는 시간은 0 이상 1,000 이하입니다.
- 각 작업에 대해 작업의 소요시간은 1 이상 1,000 이하입니다.
- 하드디스크가 작업을 수행하고 있지 않을 때에는 먼저 요청이 들어온 작업부터 처리합니다.

##### 입출력 예

|jobs|return|
|---|---|
|[[0, 3], [1, 9], [2, 6]]|9|

##### 입출력 예 설명

문제에 주어진 예와 같습니다.

- 0ms 시점에 3ms 걸리는 작업 요청이 들어옵니다.
- 1ms 시점에 9ms 걸리는 작업 요청이 들어옵니다.
- 2ms 시점에 6ms 걸리는 작업 요청이 들어옵니다.


---

### 💡 풀이

- Job Scheduling 이라는 그리디 알고리즘의 일종인 문제입니다.
- 조건을 따져서 각 시점에서 가장 빨리 끝나는 작업을 찾으면 되는데 이 조건을 파악하는 게 전부입니다. 
- 한 번 중심을 잃으면 시간을 많이 빼앗길 수 있는 문제이기 때문에 우선 쉽게 파악할 수 있는 조건부터 살펴보았습니다.
- **\[작업이 요청되는 시점, 소요시간\]** 이 주어지는데 작업이 요청되는 시점이 더 빠른 요청이 나중에 처리된다면 뒤로 밀린 시간만큼 손해를 보기 때문에 우선은 작업 요청이 빠른 순서로 정렬을 해야 합니다.

```java
Arrays.sort(jobs, (o1, o2) -> o1[0] - o2[0]);
```

- 그리고 만약 작업이 요청되는 시간이 같다면 더 빨리 끝나는 작업을 위주로 처리하면 되겠죠! 여기까지는 직관적으로 파악할 수 있었습니다.

```java
PriorityQueue<Job> pq = new PriorityQueue<>((o1, o2) -> o1.duration - o2.duration);

public class Job {
	int start;
	int duration;
	
	public Job(int start, int duration) {
		this.start = start;
		this.duration = duration;
	}
}
```

- 예제를 살펴보면, \[0, 3\] 이 가장 처음에 처리되고 그 다음에 더 빠른 요청 시간인 \[1, 9\] 가 처리되어야 할 것 같은데 오히려 \[2, 6\] 을 먼저 처리하는 게 더 빠른 시간이 됩니다.
- 이 차이를 알아내는 것이 관건인 문제였습니다 ㅠㅠ
- 먼저 \[0, 3\] 이 처리되었을 때의 종료 시점은 3이 되고, 그 다음으로 오는 후보들은 우선 3이내에 요청이 있는 것들이어야 합니다. 만약 5라는 시점에 요청이 있다면 3~5 사이 동안 무의미한 대기 시간이 생기기 때문입니다. 물론 요청 시간이 5가 가장 작다면 5가 우선입니다.
- 따라서 여러 경우의 수를 차치하고 **종료 시점 이전에 요청이 들어오는 것들을 우선 처리**한다고 생각을 합니다.
- 그리고 **이 요청들 중에 소요 시간이 더 적은 요청을 우선 처리**해주는 것이 베스트입니다.
- 그 이유는 계산식을 생각해보면 알 수 있는데요!
- 요청에서 종료까지를 계산하는 식을 보면 **처리가 끝난 시점 - 요청 시간** 입니다.  처리가 끝나는 시점이 길어질 수록 시간이 더 길어지죠! 
- 이전 태스크까지 마친 시점이 3이었다면 **3 + 소요시간이 처리가 끝난 시점**이 되는데 이 소요시간을 짧게 하는 것. 그러니까 소요시간이 더 적은 요청을 우선 처리하는 게 조건에 부합하는 것이 됩니다!


### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(int[][] jobs) {
        Arrays.sort(jobs, (o1, o2) -> o1[0] - o2[0]);
        PriorityQueue<Job> pq = new PriorityQueue<>((o1, o2) -> o1.duration - o2.duration);
        
        int total = 0;
        int end = 0;
        int i = 0;
        while (i < jobs.length || !pq.isEmpty()) {
            while (i < jobs.length && jobs[i][0] <= end) {
                pq.add(new Job(jobs[i][0], jobs[i][1]));
                i++;
            }
            
            if (pq.isEmpty()) {
                end = jobs[i][0];
            } else {
                Job current = pq.poll();
                end += current.duration;
                total += end - current.start;
            }
        }
        
        return total / jobs.length;
    }
    
    public class Job {
        int start;
        int duration;
        
        public Job(int start, int duration) {
            this.start = start;
            this.duration = duration;
        }
    }
}
```