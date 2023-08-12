# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42587/solution_groups?language=java&type=my)

## 📝 문제

운영체제의 역할 중 하나는 컴퓨터 시스템의 자원을 효율적으로 관리하는 것입니다. 이 문제에서는 운영체제가 다음 규칙에 따라 프로세스를 관리할 경우 특정 프로세스가 몇 번째로 실행되는지 알아내면 됩니다.

```
1. 실행 대기 큐(Queue)에서 대기중인 프로세스 하나를 꺼냅니다.
2. 큐에 대기중인 프로세스 중 우선순위가 더 높은 프로세스가 있다면 방금 꺼낸 프로세스를 다시 큐에 넣습니다.
3. 만약 그런 프로세스가 없다면 방금 꺼낸 프로세스를 실행합니다.
  3.1 한 번 실행한 프로세스는 다시 큐에 넣지 않고 그대로 종료됩니다.
```

예를 들어 프로세스 4개 [A, B, C, D]가 순서대로 실행 대기 큐에 들어있고, 우선순위가 [2, 1, 3, 2]라면 [C, D, A, B] 순으로 실행하게 됩니다.

현재 실행 대기 큐(Queue)에 있는 프로세스의 중요도가 순서대로 담긴 배열 `priorities`와, 몇 번째로 실행되는지 알고싶은 프로세스의 위치를 알려주는 `location`이 매개변수로 주어질 때, 해당 프로세스가 몇 번째로 실행되는지 return 하도록 solution 함수를 작성해주세요.

---

##### 제한사항

- `priorities`의 길이는 1 이상 100 이하입니다.
    - `priorities`의 원소는 1 이상 9 이하의 정수입니다.
    - `priorities`의 원소는 우선순위를 나타내며 숫자가 클 수록 우선순위가 높습니다.
- `location`은 0 이상 (대기 큐에 있는 프로세스 수 - 1) 이하의 값을 가집니다.
    - `priorities`의 가장 앞에 있으면 0, 두 번째에 있으면 1 … 과 같이 표현합니다.

##### 입출력 예

|priorities|location|return|
|---|---|---|
|[2, 1, 3, 2]|2|1|
|[1, 1, 9, 1, 1, 1]|0|5|

##### 입출력 예 설명

예제 #1

문제에 나온 예와 같습니다.

예제 #2

6개의 프로세스 [A, B, C, D, E, F]가 대기 큐에 있고 중요도가 [1, 1, 9, 1, 1, 1] 이므로 [C, D, E, F, A, B] 순으로 실행됩니다. 따라서 A는 5번째로 실행됩니다.

---

### 💡 풀이

- 우선 각 프로세스에 value와 초기 index값을 저장합니다.
- \[2, 1, 3, 2\] 라면 Process1 = \[2, 0\], Process2 = \[1, 1\], Process3 = \[3, 2\], Process4 = \[2, 3\]  이 되겠죠!
- 그러면 큐에 각 프로세스를 담아두고 하나씩 꺼내서 큐에 남아있는 수들보다 같거나 크다면 실행해주고 그렇지 않다면 다시 큐에 집어넣으면 되겠네요!
- 그런데 현재 꺼낸 프로세스가 큐에 남아있는 수들보다 크거나 같은지를 어떻게 확인할 수 있을까요?
- 저는 문제의 조건 중 value가 1~9에 한정되어있다는 것을 이용하였습니다.
- numbers라는 배열을 선언하고 각 value의 개수를 넣어놓은 것이죠! 예를 들면 \[2, 1, 3, 2\] 의 경우 numbers = \[0, 1, 2, 1, 0, 0, 0, 0, 0, 0\] 가 됩니다. 1이 1개이니까 1번 인덱스가 1, 2가 2개니까 2번 인덱스가 2, 3은 1개이니까 3번 인덱스가 1 나머지는 0이 되는 식입니다.
- 그런 후에 인덱스 뒤부터 0이 아닌 경우 현재 value값과 numbers의 인덱스 값을 비교하면 현재 프로세스가 가장 큰 수인지를 확인할 수 있습니다!

```java
int[] numbers = new int[10];
for (int p : priorities) {
	numbers[p]++;
}

						.
						.
						.

public boolean isBiggest(int num, int[] numbers) {
	for (int i = numbers.length-1; i >= 0; i--) {
		if (numbers[i] > 0 && num < i) return false;
	}
	return true;
}
```


### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(int[] priorities, int location) {
        int[] numbers = new int[10];
        for (int p : priorities) {
            numbers[p]++;
        }

        Queue<Process> processQ = new LinkedList<>();
        for (int i = 0; i < priorities.length; i++) {
            processQ.offer(new Process(priorities[i], i));
        }

        int order = 1;
        while (true) {
            Process current = processQ.poll();

            if (isBiggest(current.value, numbers)) {
                if (current.index == location) break;
                else {
                    numbers[current.value]--;
                    order++;
                    continue;
                }
            }
            processQ.offer(current);
        }

        return order;
    }
    public boolean isBiggest(int num, int[] numbers) {
        for (int i = numbers.length-1; i >= 0; i--) {
            if (numbers[i] > 0 && num < i) return false;
        }
        return true;
    }

    public class Process {
        int value;
        int index;

        public Process(int value, int index) {
            this.value = value;
            this.index = index;
        }
    }
}
```