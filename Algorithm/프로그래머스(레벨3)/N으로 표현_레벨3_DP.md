# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42895)

## 📝 문제

아래와 같이 5와 사칙연산만으로 12를 표현할 수 있습니다.

12 = 5 + 5 + (5 / 5) + (5 / 5)  
12 = 55 / 5 + 5 / 5  
12 = (55 + 5) / 5

5를 사용한 횟수는 각각 6,5,4 입니다. 그리고 이중 가장 작은 경우는 4입니다.  
이처럼 숫자 N과 number가 주어질 때, N과 사칙연산만 사용해서 표현 할 수 있는 방법 중 N 사용횟수의 최솟값을 return 하도록 solution 함수를 작성하세요.

##### 제한사항

- N은 1 이상 9 이하입니다.
- number는 1 이상 32,000 이하입니다.
- 수식에는 괄호와 사칙연산만 가능하며 나누기 연산에서 나머지는 무시합니다.
- 최솟값이 8보다 크면 -1을 return 합니다.

##### 입출력 예

|N|number|return|
|---|---|---|
|5|12|4|
|2|11|3|

##### 입출력 예 설명

예제 #1  
문제에 나온 예와 같습니다.

예제 #2  
`11 = 22 / 2`와 같이 2를 3번만 사용하여 표현할 수 있습니다.

---

### 💡 풀이

- DFS로 접근을 해보았었는데 괄호에 따른 계산을 구현하기가 어려웠습니다 ㅠㅠ
- 오답이어도 시간 초과가 뜨지 않아서 약간의 로직만 수정하면 되지 않을까 해서 찾아보았는데 완전히 다른 접근을 하고 있었다는 걸 깨달았네요!
- 이전에 DFS로 풀이를 한 사람도 있었는데 테스트 케이스가 추가되면서 DP로만 해결되도록 바뀐 것 같습니다.

### ❌ 오답

```java
import java.util.*;

class Solution {
    static String[] operator = {"+", "-", "*", "/"};
    static int N, number;
    static int min = -1;
    
    public int solution(int N, int number) {
        this.N = N;
        this.number = number;
        
        for (int i = 1; i < 8; i++) {
            calculate(N, i, 1);
        }
     
        return min;
    }
    
    public void calculate(int value, int count, int depth) {
        if (min > 0) return;
        
        if (count == depth) {
            if (value == number) min = count;
            return;
        }
        
        for (int i = 0; i < operator.length; i++) {
            calculate(concatNumber(depth), count, depth+1);
            calculate(operation(value, operator[i]), count, depth+1);
        }
    }
    
    public int concatNumber(int depth) {
        int value = N;
        for (int i = 0; i < depth; i++) {
            value = value * 10 + N;
        }
        return value;
    }
    
    public int operation(int value, String operator) {
        switch (operator) {
            case "+": value += N;
                break;
            case "-": value -= N;
                break;
            case "*": value *= N;
                break;
            case "/": value /= N;
                break;
        }       
        return value;
    }
}
```

### 🔍 정답

```java
import java.util.*;

class Solution {
    static List<HashSet<Integer>> set = new ArrayList<>();
    static int N;
    
    public int solution(int N, int number) {
        this.N = N;
        
        for (int i = 0; i < 9; i++) {
            set.add(new HashSet<>());
        }
        
        set.get(1).add(N);
        
        for (int i = 1; i < 9; i++) {
            set.get(i).add(concatNumber(i));
            
			for (int j = 1; j < i; j++) {
				calculate(i, set.get(i-j), set.get(j));
			}
            
            if (set.get(i).contains(number)) return i;
        }
     
        return -1;
    }
    
    public void calculate(int count, HashSet<Integer> set1, HashSet<Integer> set2) {
        for (int s1 : set1) {
            for (int s2 : set2) {
                set.get(count).add(s1 + s2);
                set.get(count).add(s1 - s2);
                set.get(count).add(s1 * s2);
                if (s2 != 0) set.get(count).add(s1 / s2);
            }
        }
    }
    
    public int concatNumber(int depth) {
        int value = N;
        while (--depth > 0) {
            value = value * 10 + N;
        }
        return value;
    }
}
```

- 처음에 풀었던 DFS 방식은 N 부터 시작해서 NN 을 포함하여 사칙연산을 하고, 그 계산된 값으로 다시 N을 이어붙이고, 사칙연산을 하는 방식입니다. 
- 깊이가 4만 되어도 경우의 수가 엄청나게 많아지게 되죠!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fk5tnY%2FbtsBlcQrQLA%2Ft9TWZbj6FZZwlJnKsetDS0%2Fimg.png)

- DP로는 위 그림과 같이 접근할 수 있습니다.
- 만약 깊이가 3이라면, dp\[1\] 에서 구한 값들과 dp\[2\] 에서 구한 값들을 서로 사칙연산 해주면 되는 것이죠!
- 만약 깊이가 5라면? 1+4, 2+3, 3+2, 4+1 처럼 깊이를 만들 수 있는 수들의 dp값들을 사칙연산 해주면 됩니다.
- 이 때 2+3 과 3+2 의 차이는 5 * 5 + 5 와 5 * (5 + 5)의 차이로 보면 될 것 같습니다. 괄호연산을 구현한 것이라고 보는 거에요!
- 그리고 중복 연산을 제거하기 위해서 Set 자료 구조를 사용합니다.
- 하나씩 순서대로 구현을 해보면, 먼저 Set은 깊이에 대한 Set입니다. 그러니까 N을 하나만 사용할 때는 Set 1번에 가능한 수를 모두 넣을 것이고 N을 두 개 사용할 때는 Set 2번에 가능한 수들을 모두 넣을 것입니다.

```java
static List<HashSet<Integer>> set = new ArrayList<>();
for (int i = 0; i < 9; i++) {
	set.add(new HashSet<>());
}
set.get(1).add(N);
```

- 위와 같이 해시셋 리스트를 만들어주는 것이죠!
- N 을 하나만 사용할 때는 가능한 수가 N 하나뿐이니까 미리 넣어줍니다.

```java
for (int i = 1; i < 9; i++) {
	set.get(i).add(concatNumber(i));
	
	for (int j = 1; j < i; j++) {
		calculate(i, set.get(i-j), set.get(j));
	}
	
	if (set.get(i).contains(number)) return i;
}

public void calculate(int count, HashSet<Integer> set1, HashSet<Integer> set2) {
	for (int s1 : set1) {
		for (int s2 : set2) {
			set.get(count).add(s1 + s2);
			set.get(count).add(s1 - s2);
			set.get(count).add(s1 * s2);
			if (s2 != 0) set.get(count).add(s1 / s2);
		}
	}
}
```

- 그리고 최솟값이 8보다 크다면 더이상 연산할 필요없이 -1을 리턴하면 되므로 1~8까지만 탐색을 할 것입니다.
- 또한 앞서 말했던 깊이가 5일 때 2+3, 3+2 를 구현하기 위해서 j값의 범위를 지정해줍니다. 5를 만들 수 있는 수는 (5-1) + 1, (5-2) + 2, (5-3) + 3 . . . 이런 식으로 구할 수가 있겠죠!
- 두 셋을 넘긴 후 사칙연산을 하고 해당 깊이에 원하는 값(number)이 있는지 확인하면 됩니다!