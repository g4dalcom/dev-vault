# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/64064)

## 📝 문제

개발팀 내에서 이벤트 개발을 담당하고 있는 "무지"는 최근 진행된 카카오이모티콘 이벤트에 비정상적인 방법으로 당첨을 시도한 응모자들을 발견하였습니다. 이런 응모자들을 따로 모아 `불량 사용자`라는 이름으로 목록을 만들어서 당첨 처리 시 제외하도록 이벤트 당첨자 담당자인 "프로도" 에게 전달하려고 합니다. 이 때 개인정보 보호을 위해 사용자 아이디 중 일부 문자를 '*' 문자로 가려서 전달했습니다. 가리고자 하는 문자 하나에 '*' 문자 하나를 사용하였고 아이디 당 최소 하나 이상의 '*' 문자를 사용하였습니다.  
"무지"와 "프로도"는 불량 사용자 목록에 매핑된 응모자 아이디를 `제재 아이디` 라고 부르기로 하였습니다.

예를 들어, 이벤트에 응모한 전체 사용자 아이디 목록이 다음과 같다면

|응모자 아이디|
|---|
|frodo|
|fradi|
|crodo|
|abc123|
|frodoc|

다음과 같이 불량 사용자 아이디 목록이 전달된 경우,

|불량 사용자|
|---|
|fr*d*|
|abc1**|

불량 사용자에 매핑되어 당첨에서 제외되어야 야 할 제재 아이디 목록은 다음과 같이 두 가지 경우가 있을 수 있습니다.

|제재 아이디|
|---|
|frodo|
|abc123|

|제재 아이디|
|---|
|fradi|
|abc123|

이벤트 응모자 아이디 목록이 담긴 배열 user_id와 불량 사용자 아이디 목록이 담긴 배열 banned_id가 매개변수로 주어질 때, 당첨에서 제외되어야 할 제재 아이디 목록은 몇가지 경우의 수가 가능한 지 return 하도록 solution 함수를 완성해주세요.

#### **[제한사항]**

- user_id 배열의 크기는 1 이상 8 이하입니다.
- user_id 배열 각 원소들의 값은 길이가 1 이상 8 이하인 문자열입니다.
    - 응모한 사용자 아이디들은 서로 중복되지 않습니다.
    - 응모한 사용자 아이디는 알파벳 소문자와 숫자로만으로 구성되어 있습니다.
- banned_id 배열의 크기는 1 이상 user_id 배열의 크기 이하입니다.
- banned_id 배열 각 원소들의 값은 길이가 1 이상 8 이하인 문자열입니다.
    - 불량 사용자 아이디는 알파벳 소문자와 숫자, 가리기 위한 문자 '*' 로만 이루어져 있습니다.
    - 불량 사용자 아이디는 '*' 문자를 하나 이상 포함하고 있습니다.
    - 불량 사용자 아이디 하나는 응모자 아이디 중 하나에 해당하고 같은 응모자 아이디가 중복해서 제재 아이디 목록에 들어가는 경우는 없습니다.
- 제재 아이디 목록들을 구했을 때 아이디들이 나열된 순서와 관계없이 아이디 목록의 내용이 동일하다면 같은 것으로 처리하여 하나로 세면 됩니다.

---

##### **[입출력 예]**

|user_id|banned_id|result|
|---|---|---|
|`["frodo", "fradi", "crodo", "abc123", "frodoc"]`|`["fr*d*", "abc1**"]`|2|
|`["frodo", "fradi", "crodo", "abc123", "frodoc"]`|`["*rodo", "*rodo", "******"]`|2|
|`["frodo", "fradi", "crodo", "abc123", "frodoc"]`|`["fr*d*", "*rodo", "******", "******"]`|3|

##### **입출력 예에 대한 설명**

##### **입출력 예 #1**

문제 설명과 같습니다.

##### **입출력 예 #2**

다음과 같이 두 가지 경우가 있습니다.

|제재 아이디|
|---|
|frodo|
|crodo|
|abc123|

|제재 아이디|
|---|
|frodo|
|crodo|
|frodoc|

##### **입출력 예 #3**

다음과 같이 세 가지 경우가 있습니다.

|제재 아이디|
|---|
|frodo|
|crodo|
|abc123|
|frodoc|

|제재 아이디|
|---|
|fradi|
|crodo|
|abc123|
|frodoc|

|제재 아이디|
|---|
|fradi|
|frodo|
|abc123|
|frodoc|

---

### 💡 풀이

- 배열의 크기와 각 원소들의 길이가 짧기 때문에 완전 탐색으로 해도 괜찮을 거라고 생각을 했고 그 외 문자열을 비교할 수 있는 방법이 떠오르지 않아서 DFS로 먼저 접근을 해보았습니다.

#### 문자열 비교

```java
public boolean matchId(String user, String banned) {
	if (user.length() != banned.length()) return false;
	
	for (int i = 0; i < user.length(); i++) {
		if (banned.charAt(i) != '*' && banned.charAt(i) != user.charAt(i)) {
			return false;
		}
	}
	return true;
}
```

- 문자열을 비교하는 로직은, 우선 문자열의 길이가 같은지를 확인하고,  * 은 건너뛰고 같은 인덱스끼리 비교하는 식입니다.
- 어떠한 알고리즘이 딱히 들어가는 게 아니라서 더 좋은 방법이 있지 않을까 하고 다른 사람의 풀이를 보았는데 저와 거의 유사한 로직을 사용하고 있었습니다.

#### 동일한 목록 중복 제거

- \[A, B, C\] 와 \[B, A, C\] 를 같은 것으로 보고 하나로 세어주어야 하므로 중복을 제거하는 로직이 있어야 하는데 이것은 TreeSet으로 구현하였습니다.
- 트리셋은 요소를 정렬해줌과 동시에 중복도 제거해주기 때문입니다.
- 그리고 트리셋으로 구성된 요소를 해시셋에 넣으면 중복요소 없이 구성이 되겠죠?!

#### 에러 핸들링

- 위와 같은 방법으로 풀이를 하고 제출을 했을 때 하나의 테스트 케이스만 통과를 하지 못해서 한참동안 원인을 찾아야 했습니다 ㅠㅠ
- 오답과 정답은 단 한 줄 차이였는데요...

```java
if (depth == banned.length) {
	// 기존
	// set.add(list);
	set.add(new TreeSet(list));
	return;
}
```

- 결과셋에 리스트를 넣는 과정에서 리스트를 그대로 넣었을 때 리턴된 이후에 아래와 같은 과정을 거치게 되는데요!

```java
for (int i = 0; i < user.length; i++) {
	if (!list.contains(user[i])) {
		if (matchId(user[i], banned[depth])) {
			list.add(user[i]);
			dfs(user, banned, depth+1);
			list.remove(user[i]);
		}
	}
}
```

- 재귀 특성상 요소가 계속 바뀌게 되는데(add, remove) 우리가 해시셋에 넣은 list를 그대로 사용하고 있죠? 저 list는 계속해서 같은 참조 주소값을 바라보면서 요소를 바꾸고 있습니다..

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvEXCw%2FbtsAOJWaAC0%2FmWdOkXNEET2iotW1zLVud1%2Fimg.png)

- 위 이미지를 보면, 셋에 넣은 상태에서 리스트 요소를 remove를 하니까 set은 건드리지 않았는데 요소가 변화를 했죠?
- set이 가리키고 있는 요소의 메모리 주소는 항상 같은데 요소의 변화가 있으니 당연히 셋에도 변화가 생긴 것입니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FQFaAN%2FbtsAKUksusi%2FlEbrVfiD6WJaYKMNKPPIB1%2Fimg.png)

- 반면 셋에 넣어줄 때마다 새로운 리스트를 생성해서 넣어주면(깊은 복사) 기존 list를 가리키던 참조 주솟값을 끊어내고 저장을 하는 것입니다.
- 우리가 사용하는 list는 하나이지만 저장할 때 새롭게 복사해서 넣는 것이라고 생각하면 될 것 같아요!

### 🔍 정답

```java
import java.util.*;

class Solution {
    static Set<Set<String>> set = new HashSet<>();
    static Set<String> list = new TreeSet<>();

    public int solution(String[] user_id, String[] banned_id) {
        dfs(user_id, banned_id, 0);

        return set.size();
    }

    public void dfs(String[] user, String[] banned, int depth) {
        if (depth == banned.length) {
            set.add(new TreeSet(list));
            return;
        }

        for (int i = 0; i < user.length; i++) {
            if (!list.contains(user[i])) {
                if (matchId(user[i], banned[depth])) {
                    list.add(user[i]);
                    dfs(user, banned, depth+1);
                    list.remove(user[i]);
                }
            }
        }
    }

    public boolean matchId(String user, String banned) {
        if (user.length() != banned.length()) return false;

        for (int i = 0; i < user.length(); i++) {
            if (banned.charAt(i) != '*' && banned.charAt(i) != user.charAt(i)) {
                return false;
            }
        }
        return true;
    }
}
```
