# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/43163)

## 📝 문제

두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.

```
1. 한 번에 한 개의 알파벳만 바꿀 수 있습니다.
2. words에 있는 단어로만 변환할 수 있습니다.
```

예를 들어 begin이 "hit", target가 "cog", words가 ["hot","dot","dog","lot","log","cog"]라면 "hit" -> "hot" -> "dot" -> "dog" -> "cog"와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 각 단어는 알파벳 소문자로만 이루어져 있습니다.
- 각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
- words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
- begin과 target은 같지 않습니다.
- 변환할 수 없는 경우에는 0를 return 합니다.

##### 입출력 예

|begin|target|words|return|
|---|---|---|---|
|"hit"|"cog"|["hot", "dot", "dog", "lot", "log", "cog"]|4|
|"hit"|"cog"|["hot", "dot", "dog", "lot", "log"]|0|

##### 입출력 예 설명

예제 #1  
문제에 나온 예와 같습니다.

예제 #2  
target인 "cog"는 words 안에 없기 때문에 변환할 수 없습니다.

---

### 💡 풀이

- 두 가지 방법으로 풀어보았습니다.
- 기본적으로 Queue를 이용한 BFS 풀이이고, 하나는 dp배열을 이용한 것이고 다른 하나는 클래스를 이용한 풀이입니다.
- 두 방법 모두 풀이의 흐름은 같습니다.
- begin에서 변할 수 있는 문자열(isPossibleConversion)을 모두 큐에 넣어주고 해당 큐에 있는 문자열은 begin에서 1번째로 변화한 것이므로 카운트를 1로 지정해주는 것이고
- 큐에서 빼낸 문자열이 변할 수 있는 다른 문자열들을 다시 큐에 넣어주면서 **이전 문자열의 카운트 + 1**을 해주는 것입니다.
- dp 배열을 이용할 경우 이전 문자열을 알기 위해서는 **indexOf** 메소드를 이용하여야 하고 indexOf 메소드는 String이나 List에서만 사용할 수 있기 때문에 String[] 에서 사용하려면 List로 변환하는 작업이 필요합니다.

```java
int idx = Arrays.asList(words).indexOf(current);
```

- 그리고 begin의 경우는 words에 존재하지 않기 때문에 idx가 -1이 되는데 이 경우를 예외처리 해주는 작업 또한 필요합니다.

```java
if (dp[i] == 0 && isPossibleConversion(current, words[i])) {
                    q.add(words[i]);
                    dp[i] = idx == -1 ? 1 : dp[idx] + 1;
```

- idx == -1 이라면 begin의 다음 문자열인 경우이므로 dp[i]를 1로 초기화해주는 것입니다.
- 위와 같은 이유 때문에 클래스를 이용한 풀이가 좀 더 깔끔하다고 생각합니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(String begin, String target, String[] words) {
        int[] dp = new int[words.length];
        
        bfs(begin, target, words, dp);
        
        int index = Arrays.asList(words).indexOf(target);
        return index == -1 ? 0 : dp[index];
    }
    
    public void bfs(String begin, String target, String[] words, int[] dp) {
        Queue<String> q = new LinkedList<>();
        q.add(begin);
        
        while (!q.isEmpty()) {
            String current = q.poll();
            int idx = Arrays.asList(words).indexOf(current);
            
            if (current.equals(target)) break;
            
            for (int i = 0; i < words.length; i++) {
                if (dp[i] == 0 && isPossibleConversion(current, words[i])) {
                    q.add(words[i]);
                    dp[i] = idx == -1 ? 1 : dp[idx] + 1;
                }
            }
        }
    }
    
    public boolean isPossibleConversion(String current, String word) {
        int cnt = 0;
        for (int i = 0; i < current.length(); i++) {
            if (current.charAt(i) != word.charAt(i)) cnt++;
        }
        return cnt == 1 ? true : false;
    }
}
```


### 🔍 정답

```java
import java.util.*;

class Solution {
    public int solution(String begin, String target, String[] words) {
        int count = 0;
        boolean[] visit = new boolean[words.length];
        Queue<Node> q = new LinkedList<>();
        q.add(new Node(begin, 0));
        
        while (!q.isEmpty()) {
            Node node = q.poll();
            
            if (node.word.equals(target)) {
                count = node.count;
                break;
            }
            
            for (int i = 0; i < words.length; i++) {
                if (!visit[i] && isPossibleConversion(node.word, words[i])) {
                    q.add(new Node(words[i], node.count+1));
                    visit[i] = true;
                }
            }
        }
        
        return count;
    }
    
    public boolean isPossibleConversion(String current, String word) {
        int cnt = 0;
        for (int i = 0; i < current.length(); i++) {
            if (current.charAt(i) != word.charAt(i)) cnt++;
        }
        return cnt == 1;
    }
    
    public class Node {
        String word;
        int count;
        
        public Node(String word, int count) {
            this.word = word;
            this.count = count;
        }
    }
}
```