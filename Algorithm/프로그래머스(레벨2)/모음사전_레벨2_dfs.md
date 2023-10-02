# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/84512)

## 📝 문제

사전에 알파벳 모음 'A', 'E', 'I', 'O', 'U'만을 사용하여 만들 수 있는, 길이 5 이하의 모든 단어가 수록되어 있습니다. 사전에서 첫 번째 단어는 "A"이고, 그다음은 "AA"이며, 마지막 단어는 "UUUUU"입니다.

단어 하나 word가 매개변수로 주어질 때, 이 단어가 사전에서 몇 번째 단어인지 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

- word의 길이는 1 이상 5 이하입니다.
- word는 알파벳 대문자 'A', 'E', 'I', 'O', 'U'로만 이루어져 있습니다.

---

##### 입출력 예

|word|result|
|---|---|
|`"AAAAE"`|6|
|`"AAAE"`|10|
|`"I"`|1563|
|`"EIO"`|1189|

##### 입출력 예 설명

입출력 예 #1

사전에서 첫 번째 단어는 "A"이고, 그다음은 "AA", "AAA", "AAAA", "AAAAA", "AAAAE", ... 와 같습니다. "AAAAE"는 사전에서 6번째 단어입니다.

입출력 예 #2

"AAAE"는 "A", "AA", "AAA", "AAAA", "AAAAA", "AAAAE", "AAAAI", "AAAAO", "AAAAU"의 다음인 10번째 단어입니다.

입출력 예 #3

"I"는 1563번째 단어입니다.

입출력 예 #4

"EIO"는 1189번째 단어입니다.

---

### 💡 풀이

- 일반적인 DFS 문제입니다.
- word의 길이는 최대 5이므로 depth를 5로 제한해서 depth == 5일 때 리턴하도록 하였습니다.
- 또한 AAAAA 로 최대 길이가 되었을 때 마지막 부분이 E, I, O, U 순서로 바뀌고 그 다음에 AAAE로 길이가 하나 줄어드는 부분은 마지막 인덱스 재귀가 끝난 후 StringBuilder의 마지막 문자를 삭제하는 것으로 구현하였습니다.
- 마지막으로 주어진 문자의 순서를 구했을 때 boolean값을 true로 바꾸어줌으로써 더이상 탐색하지 않고 종료되도록 하였습니다!

### 🔍 정답

```java
class Solution {    
    static final String[] alphabet = {"A", "E", "I", "O", "U"};
    static StringBuilder sb = new StringBuilder();
    static int count = 0;
    static int order = 0;
    static boolean isFind = false;
    
    public int solution(String word) {
        dfs(word, 0);     
        return order;
    }
    
    public void dfs(String word, int depth) {
        System.out.println(sb.toString());
        if (sb.toString().equals(word)) {
            order = count;
            isFind = true;
            return;
        }
        
        if (depth == 5) return;
        
        for (int i = 0; i < alphabet.length; i++) {
            sb.append(alphabet[i]);
            count++;
            dfs(word, depth+1);
            
            if (isFind) return;
            
            sb.deleteCharAt(sb.length() - 1);
        }
    }
}
```