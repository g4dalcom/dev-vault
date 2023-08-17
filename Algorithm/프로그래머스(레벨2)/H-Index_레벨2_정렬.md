# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42747)

## 📝 문제

H-Index는 과학자의 생산성과 영향력을 나타내는 지표입니다. 어느 과학자의 H-Index를 나타내는 값인 h를 구하려고 합니다. 위키백과[1](https://school.programmers.co.kr/learn/courses/30/lessons/42747#fn1)에 따르면, H-Index는 다음과 같이 구합니다.

어떤 과학자가 발표한 논문 `n`편 중, `h`번 이상 인용된 논문이 `h`편 이상이고 나머지 논문이 h번 이하 인용되었다면 `h`의 최댓값이 이 과학자의 H-Index입니다.

어떤 과학자가 발표한 논문의 인용 횟수를 담은 배열 citations가 매개변수로 주어질 때, 이 과학자의 H-Index를 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 과학자가 발표한 논문의 수는 1편 이상 1,000편 이하입니다.
- 논문별 인용 횟수는 0회 이상 10,000회 이하입니다.

##### 입출력 예

|citations|return|
|---|---|
|[3, 0, 6, 1, 5]|3|

##### 입출력 예 설명

이 과학자가 발표한 논문의 수는 5편이고, 그중 3편의 논문은 3회 이상 인용되었습니다. 그리고 나머지 2편의 논문은 3회 이하 인용되었기 때문에 이 과학자의 H-Index는 3입니다.

---

### 💡 풀이

- 정렬을 단순히 `Arrays.sort(citations)` 로 할 수 있지만 연습겸 퀵정렬로도 구현해보았습니다.
- 처음 문제를 이해했을 때는 h번 이상 인용된 논문이 h 이상 존재해야 하기 때문에 아래와 같이 구하였습니다.

```java
for (int i = 0; i < citations.length; i++) {
	int current = citations.length - i;
	if (citations[i] <= current) {
		answer = current;
	}
}
```

- \[3, 0, 6, 1, 5\] 를 정렬해서 \[0, 1, 3, 5, 6\] 을 얻었다고 가정했을 때,
	- 0번 이상 인용된 논문이 총 5개
	- 1번 이상 인용된 논문이 총 4개
	- 3번 이상 인용된 논문이 총 3개
	- 5번 이상 인용된 논문은 총 2개(5, 6) 이므로 오답
	- 6번 이상 인용된 논문은 총 1개이므로 오답
- 따라서 0, 1, 3이 조건에 해당하는데 가장 큰 수인 3이 H-Index라고 생각하였습니다.
- 그런데 테스트케이스만 통과되고 제출을 했더니 대부분이 오답이 떠서 한참을 고민하다가 다른 답안을 찾아보았고 이 문제의 설명 자체가 H-Index에 대해 설명이 빈약하다는 것을 알게 되었습니다.
- [H-Index 개념](https://postechlibrary.tistory.com/489)
- H-Index는 논문을 많이 인용한 순서대로 정렬을 하고 순번과 인용된 횟수를 비교하다가 **순번과 횟수가 같아**지거나, 혹은 **순번보다 횟수가 작아지기 직전의 순번**이 된다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(int[] citations) {
        int answer = 0;
        Integer[] c = Arrays.stream(citations).boxed().toArray(Integer[]::new);
        Arrays.sort(c, Collections.reverseOrder());
        
        for (int i = 0; i < citations.length; i++) {
            if (i+1 <= c[i]) {
                answer = i+1;
            }
        }        
        return answer;
    }
}
```

### 🔍 정답

```java
class Solution {
    public int solution(int[] citations) {
        sort(citations, 0, citations.length-1);
        int answer = 0;
        
        for (int i = 0; i < citations.length; i++) {
            int current = citations.length - i;
            if (citations[i] >= current) {
                answer = current;
                break;
            }
        }
        
        return answer;
    }
    
    public void sort(int[] citations, int low, int high) {
        if (low >= high) return;
        
        int mid = partition(citations, low, high);
        sort(citations, low, mid-1);
        sort(citations, mid, high);
    }
    
    public int partition(int[] citations, int low, int high) {
        int pivot = citations[(low + high) / 2];
        while (low <= high) {
            while (citations[low] < pivot) low++;
            while (citations[high] > pivot) high--;
            if (low <= high) {
                swap(citations, low, high);
                low++;
                high--;
            }
        }
        return low;
    }
    
    public void swap(int[] citations, int i, int j) {
        int temp = citations[i];
        citations[i] = citations[j];
        citations[j] = temp;
    }
}
```