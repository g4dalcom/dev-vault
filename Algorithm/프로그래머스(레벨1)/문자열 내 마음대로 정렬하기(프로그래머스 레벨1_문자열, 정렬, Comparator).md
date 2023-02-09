# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12915)

## 📝 문제

문자열로 구성된 리스트 strings와, 정수 n이 주어졌을 때, 각 문자열의 인덱스 n번째 글자를 기준으로 오름차순 정렬하려 합니다. 예를 들어 strings가 ["sun", "bed", "car"]이고 n이 1이면 각 단어의 인덱스 1의 문자 "u", "e", "a"로 strings를 정렬합니다.

##### 제한 조건

-   strings는 길이 1 이상, 50이하인 배열입니다.
-   strings의 원소는 소문자 알파벳으로 이루어져 있습니다.
-   strings의 원소는 길이 1 이상, 100이하인 문자열입니다.
-   모든 strings의 원소의 길이는 n보다 큽니다.
-   인덱스 1의 문자가 같은 문자열이 여럿 일 경우, 사전순으로 앞선 문자열이 앞쪽에 위치합니다.

##### 입출력 예

| strings                 | n   | return                |
|:----------------------- |:--- |:--------------------- |
| ["sun", "bed", "car"]   | 1   | ["car", "bed", "sun"] |
| ["abce", "abcd", "cdx"] | 2   |     ["abcd", "abce", "cdx"]                  |


##### 입출력 예 설명

**입출력 예 1**  
"sun", "bed", "car"의 1번째 인덱스 값은 각각 "u", "e", "a" 입니다. 이를 기준으로 strings를 정렬하면 ["car", "bed", "sun"] 입니다.

**입출력 예 2**  
"abce"와 "abcd", "cdx"의 2번째 인덱스 값은 "c", "c", "x"입니다. 따라서 정렬 후에는 "cdx"가 가장 뒤에 위치합니다. "abce"와 "abcd"는 사전순으로 정렬하면 "abcd"가 우선하므로, 답은 ["abcd", "abce", "cdx"] 입니다.

---

### 🔍 정답

```java
import java.util.*;

class Solution {
    public String[] solution(String[] strings, int n) {
        String[] answer = new String[strings.length];
        String[] temp = new String[strings.length];

        for (int i = 0; i < strings.length; i++) {
            char[] toChar = strings[i].toCharArray();
            char ch = toChar[n];
            for (int j = 0; j < strings[i].length(); j++) {
                temp[i] = ch + strings[i];
            }
        }
        Arrays.sort(temp);

        for (int i = 0; i < strings.length; i++) {
            answer[i] = temp[i].substring(1);
        }
        
        return answer;
    }
}
```
- 각 문자열 맨 앞에 정렬 기준 인덱스인 문자를 붙여서 정렬한 후 다시 떼어서 반환하는 방법이다.
	- ["sun", ''bed", "car"] 를 인덱스 1번 기준으로 정렬하고자 할 때,
	-  ["acar", "ebed", "usun"] 으로 만든 후 정렬하고 0번 인덱스 문자를 떼어낸다.
- 이 방법은 정렬 기준 문자가 같을 때 자동으로 사전순으로도 정렬이 된다.


### 🔎 정답(Comparator)

```java
import java.util.*;

class Solution {
    public String[] solution(String[] strings, int n) {
        Arrays.sort(strings, new Comparator<String>() {
            @Override
            public int compare(String s1, String s2) {
                if (s1.charAt(n) == s2.charAt(n)) return s1.compareTo(s2);
                else return s1.charAt(n) - s2.charAt(n);
            }
        });
        return strings;
    }
}
```
- Comparator 메서드를 활용해서 정렬 기준 문자가 같다면 문자열을 사전순으로 정렬하고
- 그렇지 않다면 정렬 기준 문자를 비교한다.
	- s1.charAt(n) - s2.charAt(n)에서 s1이 더 크다면 양수가 나와서 둘의 자리가 바뀌고 s1이 더 작아서 음수가 나오면 위치가 그대로 유지되어서 오름차순으로 정렬이 된다.