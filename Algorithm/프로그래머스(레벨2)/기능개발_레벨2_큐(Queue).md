# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42586)

## 📝 문제

프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

##### 제한 사항

- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

##### 입출력 예

|progresses|speeds|return|
|---|---|---|
|[93, 30, 55]|[1, 30, 5]|[2, 1]|
|[95, 90, 99, 99, 80, 99]|[1, 1, 1, 1, 1, 1]|[1, 3, 2]|

##### 입출력 예 설명

입출력 예 #1  
첫 번째 기능은 93% 완료되어 있고 하루에 1%씩 작업이 가능하므로 7일간 작업 후 배포가 가능합니다.  
두 번째 기능은 30%가 완료되어 있고 하루에 30%씩 작업이 가능하므로 3일간 작업 후 배포가 가능합니다. 하지만 이전 첫 번째 기능이 아직 완성된 상태가 아니기 때문에 첫 번째 기능이 배포되는 7일째 배포됩니다.  
세 번째 기능은 55%가 완료되어 있고 하루에 5%씩 작업이 가능하므로 9일간 작업 후 배포가 가능합니다.

따라서 7일째에 2개의 기능, 9일째에 1개의 기능이 배포됩니다.

입출력 예 #2  
모든 기능이 하루에 1%씩 작업이 가능하므로, 작업이 끝나기까지 남은 일수는 각각 5일, 10일, 1일, 1일, 20일, 1일입니다. 어떤 기능이 먼저 완성되었더라도 앞에 있는 모든 기능이 완성되지 않으면 배포가 불가능합니다.

따라서 5일째에 1개의 기능, 10일째에 3개의 기능, 20일째에 2개의 기능이 배포됩니다.

---

### 💡 풀이

- 매일 모든 기능을 speed 만큼 구현합니다. 
- 원래 percent가 100이 되면 기능 구현을 종료해야 하지만 이 문제에서는 고려하지 않아도 되었기 때문에 앞 기능이 완료되지 않으면 percent는 100을 초과해도 계속 늘어납니다.
- 그리고 가장 앞 기능 **waitQ.peek().percent** 가 100이 넘어가는 순간 카운팅을 하고 poll()을 합니다.
- 그리고 다시 **waitQ.peek().percent**를 확인하고 100이 넘는다면 추가로 카운팅을 하는 식입니다.
- 풀고나서보니 기능 구현이 다 되었음에도 앞 기능이 완료되지 않으면 계속 percent가 늘어나는 게 거슬렸는데, Func class에 boolean 필드를 추가해서 percent가 100이 넘었을 때 true로 바꾸어주고 완료가 false인 경우에만 percent를 증가시키는 방법도 있을 것 같네요!

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        Queue<Func> waitQ = new LinkedList<>();
        
        for (int i = 0; i < progresses.length; i++) {
            waitQ.offer(new Func(progresses[i], speeds[i]));
        }

        ArrayList<Integer> days = new ArrayList<>();
        while (!waitQ.isEmpty()) {
            int count = 0;
            
            while (true) {
                if (waitQ.size() > 0 && waitQ.peek().percent >= 100) {
                    waitQ.poll();
                    count++;
                    continue;
                }
                
                if (count > 0) break;
                
                for (Func f : waitQ) {
                    f.progress();
                } 
            }
            days.add(count); 
        }
        
        return days.stream().mapToInt(Integer::intValue).toArray();
    }
    
    class Func {
        int percent;
        int speed;
        
        public Func(int percent, int speed) {
            this.percent = percent;
            this.speed = speed;
        }
        
        public void progress() {
            this.percent += speed;
        }
    }
}
```