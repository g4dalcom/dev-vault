# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/176962)

## 📝 문제

과제를 받은 루는 다음과 같은 순서대로 과제를 하려고 계획을 세웠습니다.

-   과제는 시작하기로 한 시각이 되면 시작합니다.
-   새로운 과제를 시작할 시각이 되었을 때, 기존에 진행 중이던 과제가 있다면 진행 중이던 과제를 멈추고 새로운 과제를 시작합니다.
-   진행중이던 과제를 끝냈을 때, 잠시 멈춘 과제가 있다면, 멈춰둔 과제를 이어서 진행합니다.
    -   만약, 과제를 끝낸 시각에 새로 시작해야 되는 과제와 잠시 멈춰둔 과제가 모두 있다면, 새로 시작해야 하는 과제부터 진행합니다.
-   멈춰둔 과제가 여러 개일 경우, 가장 최근에 멈춘 과제부터 시작합니다.

과제 계획을 담은 이차원 문자열 배열 `plans`가 매개변수로 주어질 때, 과제를 끝낸 순서대로 이름을 배열에 담아 return 하는 solution 함수를 완성해주세요.

---

##### 제한사항

-   3 ≤ `plans`의 길이 ≤ 1,000
    -   `plans`의 원소는 [name, start, playtime]의 구조로 이루어져 있습니다.
        -   name : 과제의 이름을 의미합니다.
            -   2 ≤ name의 길이 ≤ 10
            -   name은 알파벳 소문자로만 이루어져 있습니다.
            -   name이 중복되는 원소는 없습니다.
        -   start : 과제의 시작 시각을 나타냅니다.
            -   "hh:mm"의 형태로 "00:00" ~ "23:59" 사이의 시간값만 들어가 있습니다.
            -   모든 과제의 시작 시각은 달라서 겹칠 일이 없습니다.
            -   과제는 "00:00" ... "23:59" 순으로 시작하면 됩니다. 즉, 시와 분의 값이 작을수록 더 빨리 시작한 과제입니다.
        -   playtime : 과제를 마치는데 걸리는 시간을 의미하며, 단위는 분입니다.
            -   1 ≤ playtime ≤ 100
            -   playtime은 0으로 시작하지 않습니다.
        -   배열은 시간순으로 정렬되어 있지 않을 수 있습니다.
-   진행중이던 과제가 끝나는 시각과 새로운 과제를 시작해야하는 시각이 같은 경우 진행중이던 과제는 끝난 것으로 판단합니다.

---

##### 입출력 예

| plans                                                                                                                 | result                                       |
|:--------------------------------------------------------------------------------------------------------------------- |:-------------------------------------------- |
| \[\["korean", "11:40", "30"], \["english", "12:10", "20"], \["math", "12:30", "40"]]                                  | \["korean", "english", "math"]               |
| \[\["science", "12:40", "50"], \["music", "12:20", "40"], \["history", "14:00", "30"], \["computer", "12:30", "100"]] | \["science", "history", "computer", "music"] |
| \[\["aaa", "12:00", "20"], \["bbb", "12:10", "30"], \["ccc", "12:40", "10"]]                                          | \["bbb", "ccc", "aaa"]                                             |

---

##### 입출력 예 설명

입출력 예 #1

"korean", "english", "math"순으로 과제를 시작합니다. "korean" 과제를 "11:40"에 시작하여 30분 후인 "12:10"에 마치고, 즉시 "english" 과제를 시작합니다. 20분 후인 "12:30"에 "english" 과제를 마치고, 즉시 "math" 과제를 시작합니다. 40분 후인 "01:10"에 "math" 과제를 마칩니다. 따라서 "korean", "english", "math" 순으로 과제를 끝내므로 차례대로 배열에 담아 반환합니다.

---

### 🔍 정답

```java
import java.util.*;

class Solution {
    public String[] solution(String[][] plans) {
        String[][] planning = new String[plans.length][plans[0].length];
        
        // 시간을 분단위로 바꾸기
        for (int i = 0; i < plans.length; i++) {
            String[] tmp = plans[i][1].split(":");
            int times = Integer.parseInt(tmp[0]) * 60 + Integer.parseInt(tmp[1]);

            planning[i][0] = plans[i][0];
            planning[i][1] = String.valueOf(times);
            planning[i][2] = plans[i][2];
        }

        // 과제 시작 시간을 기준으로 오름차순 정렬
        Arrays.sort(planning, new Comparator<String[]>() {
            @Override
            public int compare(final String[] o1, final String[] o2) {
                return Integer.parseInt(o1[1]) - Integer.parseInt(o2[1]);
            }
        });

        // 큐에는 완료된 과제를, 스택에는 못다한 과제를 시간과 함께 저장
        Queue<String> complete = new LinkedList<>();
        Stack<Info> temp = new Stack<>();

        for (int i = 0; i < plans.length; i++) {

            if (i < plans.length - 1) {
                int cur = Integer.parseInt(planning[i][1]) + Integer.parseInt(planning[i][2]);
                int next = Integer.parseInt(planning[i+1][1]);

                if (cur == next) complete.offer(planning[i][0]);
                else if (cur < next) {
                    int gap = next - cur;
                    complete.offer(planning[i][0]);
                    
                    while (!temp.isEmpty()) {
                        Info re = temp.pop();
                        
                        if (re.remain <= gap) {
                            complete.offer(re.name);
                            gap = gap - re.remain;
                        } else {
                            temp.push(new Info(re.name, re.remain - gap));
                            break;
                        }
                    }
                } else {
                    int gap = cur - next;
                    temp.push(new Info(planning[i][0], gap));
                }
            } else {
                complete.offer(planning[i][0]);
            }
        }

        // 과제가 완료되면 스택에서 하나씩 꺼내서 완료 목록에 넘겨주기
        while (!temp.isEmpty()) complete.offer(temp.pop().name);

        String[] result = new String[plans.length];
        for (int i = 0; i < plans.length; i++) {
            result[i] = complete.poll();
        }
        
        return result;
    }

    public static class Info {
        String name;
        int remain;

        public Info (final String name, final int remain) {
            this.name = name;
            this.remain = remain;
        }
    }
}
```
- <현재 과제의 시작 시간 + 걸리는 시간>(cur) 과 <다음 과제의 시작 시간>(next)을 비교한다.
	- 만약 cur == next라면, 현재 과제를 마치고 바로 다음 과제를 진행하면 되므로 현재 과제를 완료 과제 목록(Queue)에 넣는다.
	- 만약 cur < next 라면, 현재 과제를 마치고 잔여 시간이 있으므로 이전에 마치지 못한 과제를 할 수가 있다. 따라서 Stack(잔여 과제)에 있는 것들을 꺼내서 잔여 시간동안 마칠 수 있는 과제는 완료 과제로 보내고 그렇지 않다면 시간을 잔여 과제의 시간을 갱신해서 다시 Stack에 넣어준다.
	- 만약 cur > next 라면, 현재 과제를 마치기 전에 다음 과제를 진행해야 하는 경우이므로 현재 과제와 남은 시간을 Stack에 넣어준다.
- 위 과정을 모두 거친 후 마지막 과제는 비교 대상이 없기 때문에 그대로 완료 과제에 넣어주고 Stack에 있는 것들을 순차적으로 완료 과제 목록에 넣어주었다!