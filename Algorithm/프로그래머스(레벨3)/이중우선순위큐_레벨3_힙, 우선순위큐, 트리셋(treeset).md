# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42628)

## 📝 문제

이중 우선순위 큐는 다음 연산을 할 수 있는 자료구조를 말합니다.

|명령어|수신 탑(높이)|
|---|---|
|I 숫자|큐에 주어진 숫자를 삽입합니다.|
|D 1|큐에서 최댓값을 삭제합니다.|
|D -1|큐에서 최솟값을 삭제합니다.|

이중 우선순위 큐가 할 연산 operations가 매개변수로 주어질 때, 모든 연산을 처리한 후 큐가 비어있으면 [0,0] 비어있지 않으면 [최댓값, 최솟값]을 return 하도록 solution 함수를 구현해주세요.

##### 제한사항

- operations는 길이가 1 이상 1,000,000 이하인 문자열 배열입니다.
- operations의 원소는 큐가 수행할 연산을 나타냅니다.
    - 원소는 “명령어 데이터” 형식으로 주어집니다.- 최댓값/최솟값을 삭제하는 연산에서 최댓값/최솟값이 둘 이상인 경우, 하나만 삭제합니다.
- 빈 큐에 데이터를 삭제하라는 연산이 주어질 경우, 해당 연산은 무시합니다.

##### 입출력 예

|operations|return|
|---|---|
|["I 16", "I -5643", "D -1", "D 1", "D 1", "I 123", "D -1"]|[0,0]|
|["I -45", "I 653", "D 1", "I -642", "I 45", "I 97", "D 1", "D -1", "I 333"]|[333, -45]|

##### 입출력 예 설명

입출력 예 #1

- 16과 -5643을 삽입합니다.
- 최솟값을 삭제합니다. -5643이 삭제되고 16이 남아있습니다.
- 최댓값을 삭제합니다. 16이 삭제되고 이중 우선순위 큐는 비어있습니다.
- 우선순위 큐가 비어있으므로 최댓값 삭제 연산이 무시됩니다.
- 123을 삽입합니다.
- 최솟값을 삭제합니다. 123이 삭제되고 이중 우선순위 큐는 비어있습니다.

따라서 [0, 0]을 반환합니다.

입출력 예 #2

- -45와 653을 삽입후 최댓값(653)을 삭제합니다. -45가 남아있습니다.
- -642, 45, 97을 삽입 후 최댓값(97), 최솟값(-642)을 삭제합니다. -45와 45가 남아있습니다.
- 333을 삽입합니다.

이중 우선순위 큐에 -45, 45, 333이 남아있으므로, [333, -45]를 반환합니다.


---

### 💡 풀이

- 문제에서는 최댓값과 최솟값을 각각 꺼낼 수 있어야 하는데 우선순위큐는 한쪽으로만 정렬이 되므로 Deque이라는 자료구조를 사용해야 되지 않을까 생각했었습니다.
- 거의 사용해보지 않았던 자료구조였기 때문에 용법을 좀 찾아보았는데 정렬이 된 상태를 유지하면서 자료를 저장하는 방법은 API상에서 구현이 되어있지 않은 것 같았습니다.
- 그래서 가장 직관적인 방법으로 우선순위큐를 2개 선언하고 두 개의 큐를 동기화하는 방법으로 해결하였습니다.
- 혹시나 Deque를 이용한 방법으로 풀이한 사람이 없는지 찾아보았는데 프로그래머스 풀이에서는 찾지 못했고 직접 메소드를 구현한 풀이와 treeset을 이용한 풀이를 공부할 수 있었습니다.

```java
import java.util.Collections;
import java.util.LinkedList;
import java.util.PriorityQueue;
import java.util.Queue;
class Solution {
    public int[] solution(String[] arguments) {
        MidV q = new MidV();

        for(int i = 0; i < arguments.length; i++){
            String[] commend = arguments[i].split(" ");

            int v = Integer.parseInt(commend[1]);
            if(commend[0].equals("I")){
                q.push(v);
            }else{
                switch (v){
                    case 1 : q.removeMax();
                    break;
                    case -1: q.removeMin();
                    break;
                }
            }
        }

        int[] aw = new int[]{q.getMaxValue(),q.getMinValue()};

        return aw;
    }
}

class MidV{
    private PriorityQueue<Integer> leftHeap;
    private PriorityQueue<Integer> rightHeap;

    public MidV(){
        leftHeap = new PriorityQueue<>(Collections.reverseOrder());//최대값;
        rightHeap = new PriorityQueue<>();//최소값
    }

    public void push(int v){
        leftHeap.add(v);
    }

    public void removeMax(){

        while(!rightHeap.isEmpty()){
            leftHeap.add(rightHeap.poll());
        }

        leftHeap.poll();
    }

    public void removeMin(){

        while(!leftHeap.isEmpty()){
            rightHeap.add(leftHeap.poll());
        }

        rightHeap.poll();
    }

    public int getMaxValue(){

        if(leftHeap.size() == 0 && rightHeap.size() == 0)
            return 0;

        while(!rightHeap.isEmpty()){
            leftHeap.add(rightHeap.poll());
        }

        return leftHeap.peek();
    }

    public int getMinValue(){

        if(leftHeap.size() == 0 && rightHeap.size() == 0)
            return 0;

        while(!leftHeap.isEmpty()){
            rightHeap.add(leftHeap.poll());
        }

        return rightHeap.peek();
    }

}
```

- 우선 메소드를 직접 구현한 풀이입니다.
- 두 개의 큐를 사용하는 것은 동일하지만 remove 메소드를 이용해서 동기화하는 것이 아니라 필요한 큐에 자료를 옮겨담는 식으로 구현한 것이 특징입니다.
- 저의 풀이같은 경우는 두 개의 큐에 각각 자료가 담겨있어서 입력값 X 2 만큼의 메모리를 사용하게 되고 위 풀이는 입력값만큼의 메모리만 사용하지만 최댓값과 최솟값이 번갈아입력될 경우 하나의 큐를 비워주고 다른 하나의 큐에 자료를 넣어야 하는 삽입, 삭제가 나타납니다.
- 두 형태의 성능이 궁금해서 비교를 해보았는데 저의 풀이가 근소하게, 그렇지만 유의미하지는 않은 정도로 좋게 나왔고 입력값의 크기나 형태에 따라 차이가 날 수 있으므로 크게 의미는 없어보였습니다.


```java
import java.util.*;

class Solution {
    public int[] solution(String[] arguments) {
        TreeSet<Integer> dpq = new TreeSet<>();

        for (String argument : arguments) {
            String[] arg = argument.split(" ");
            int num = Integer.parseInt(arg[1]);

            if ("I".equals(arg[0])) {
                dpq.add(num);
                continue;
            }

            if (dpq.isEmpty()) continue;

            if (num == 1) {
                dpq.pollLast();
            } else if (num == -1) {
                dpq.pollFirst();
            }
        }

        int[] answer = new int[2];

        if (dpq.size() > 1) {
            answer[0] = dpq.pollLast();
            answer[1] = dpq.pollFirst();
        } else if (dpq.size() == 1) {
            answer[0] = answer[1] = dpq.pollFirst();
        }

        return answer;
    }
}
```

- 그 다음으로는 treeset을 이용한 풀이입니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FGVg91%2FbtszJH7bbfu%2FJFK9ER9oSmF78euzZYCum0%2Fimg.png)

- 트리셋은 이진탐색트리 구조로 되어있고 그 중 레드-블랙 트리로 구현되어 있습니다.
- 이것은 부모노드보다 작은 노드는 왼쪽으로, 큰 노드는 오른쪽에 배치함으로써 데이터 추가, 삭제시 트리가 한쪽으로 치우쳐지는 것을 방지하는 자료구조입니다.
- 기본적으로 추가, 삭제는 시간이 좀 더 걸리지만 정렬이나 검색에 최적화되어 있습니다.
- pollFirst()나 pollLast() 메소드를 이용해서 정렬된 데이터에서 최댓값과 최솟값을 바로 뽑아낼 수 있습니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int[] solution(String[] operations) {
        PriorityQueue<Integer> max_pq = new PriorityQueue<>((o1, o2) -> o2 - o1);
        PriorityQueue<Integer> min_pq = new PriorityQueue<>();
        
        for (String operation : operations) {
            String[] op = operation.split(" ");
            String command = op[0];
            int value = Integer.parseInt(op[1]);
            
            if (command.equals("D")) {
                if (value == 1 && !max_pq.isEmpty()) {
                    int current = max_pq.poll();
                    min_pq.remove(current);
                } else if (value == -1 && !min_pq.isEmpty()) {
                    int current = min_pq.poll();
                    max_pq.remove(current);
                }
            } else {
                max_pq.offer(value);
                min_pq.offer(value);
            }
        }
        
        if (max_pq.isEmpty() && min_pq.isEmpty()) return new int[]{0, 0};
        else return new int[]{max_pq.poll(), min_pq.poll()};
    }
}
```