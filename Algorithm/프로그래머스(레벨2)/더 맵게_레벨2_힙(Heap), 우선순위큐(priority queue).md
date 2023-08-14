# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42626)

## 📝 문제

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

```
섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)
```

Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.  
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

##### 제한 사항

- scoville의 길이는 2 이상 1,000,000 이하입니다.
- K는 0 이상 1,000,000,000 이하입니다.
- scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
- 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

##### 입출력 예

|scoville|K|return|
|---|---|---|
|[1, 2, 3, 9, 10, 12]|7|2|

##### 입출력 예 설명

1. 스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.  
    새로운 음식의 스코빌 지수 = 1 + (2 * 2) = 5  
    가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]
    
2. 스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.  
    새로운 음식의 스코빌 지수 = 3 + (5 * 2) = 13  
    가진 음식의 스코빌 지수 = [13, 9, 10, 12]
    

모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.

---

### 💡 풀이

- 가장 스코빌 지수가 작은 음식 두 개를 매번 꺼내야하고 두 음식의 스코빌 지수를 조합한 수를 포함해서 다시 정렬을 해야하기 때문에 **우선순위 큐**를 사용했습니다. 
- 만약 계속 조합을 해도 K를 만족시키는 수가 나오지 않으면 -1을 리턴해야 하는데 이런 경우에는 큐에 하나의 음식이 남을 때까지 조합을 하고 두 번째 음식을 꺼낼 수 없는 경우에 -1을 리턴하도록 해보았습니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(int[] scoville, int K) {
        int answer = 0;
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int e : scoville) {
            pq.add(e);
        }
        
        while (true) {
            if (pq.peek() >= K) break;
            
            int least = pq.poll();
            if (pq.isEmpty()) {
                answer = -1;
                break;
            }
            
            int second = pq.poll();
            pq.add(least + (second * 2));
            answer++;
        }
        return answer;
    }
}
```


### 🔍 다른 정답

```java
import java.util.*;
class Solution {
    public int solution(int[] scoville, int K) {
        PriorityQueue<Integer> q = new PriorityQueue<>();

        for(int i = 0; i < scoville.length; i++)
            q.add(scoville[i]);

        int count = 0;
        while(q.size() > 1 && q.peek() < K){
            int weakHot = q.poll();
            int secondWeakHot = q.poll();

            int mixHot = weakHot + (secondWeakHot * 2);
            q.add(mixHot);
            count++;
        }

        if(q.size() <= 1 && q.peek() < K)
            count = -1;

        return count;
    }
}
```

- 같은 로직으로 거의 유사하게 푼 답안인데 가장 좋아요를 많이 받았습니다.
- 우선 while문에 조건과 변수명이 직관적이라서 훨씬 알아보기 좋았던 것 같습니다.
- 나름 변수명을 신경써서 least, second로 정했는데 좀 더 직관적인 변수명을 사용하도록 고민해보아야겠습니다 ㅠㅠ