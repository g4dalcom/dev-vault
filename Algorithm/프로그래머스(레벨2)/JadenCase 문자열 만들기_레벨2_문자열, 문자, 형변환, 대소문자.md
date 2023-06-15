# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/12951)

## 📝 문제

JadenCase란 모든 단어의 첫 문자가 대문자이고, 그 외의 알파벳은 소문자인 문자열입니다. 단, 첫 문자가 알파벳이 아닐 때에는 이어지는 알파벳은 소문자로 쓰면 됩니다. (첫 번째 입출력 예 참고)  
문자열 s가 주어졌을 때, s를 JadenCase로 바꾼 문자열을 리턴하는 함수, solution을 완성해주세요.

##### 제한 조건

- s는 길이 1 이상 200 이하인 문자열입니다.
- s는 알파벳과 숫자, 공백문자(" ")로 이루어져 있습니다.
    - 숫자는 단어의 첫 문자로만 나옵니다.
    - 숫자로만 이루어진 단어는 없습니다.
    - 공백문자가 연속해서 나올 수 있습니다.

##### 입출력 예

|s|return|
|---|---|
|"3people unFollowed me"|"3people Unfollowed Me"|
|"for the last week"|"For The Last Week"|

---

### 💡 풀이

- String에서 첫 글자인 경우는 String의 첫 번째 인덱스이거나 공백을 만난 다음 인덱스이다. 이 인덱스를 판별하기 위한 boolean 타입 check를 선언하였다.
- 공백을 만나면 check = true 값을 주었고
- check = true 라면 해당 인덱스는 대문자로 바꾸고 check를 다시 false로 돌려놓았다.


### 🔍 정답

```java
class Solution {
    public String solution(String s) {
        boolean check = false;
        s = s.toLowerCase();
        char[] ch = s.toCharArray();
        
        for (int i = 0; i < ch.length; i++) {
            if (ch[i] == 32) {
                check = true;
                continue;
            }
            if (i == 0 || check) ch[i] = Character.toUpperCase(ch[i]); 
            
            check = false;
        }
        
        return String.valueOf(ch);
    }
}
```


### 💡 다른 정답

```java
class Solution {
  public String solution(String s) {
        String answer = "";
        String[] sp = s.toLowerCase().split("");
        boolean flag = true;

        for(String ss : sp) {
            answer += flag ? ss.toUpperCase() : ss;
            flag = ss.equals(" ") ? true : false;
        }

        return answer;
  }
}
```

- 같은 원리이지만 훨씬 깔끔한 방법......