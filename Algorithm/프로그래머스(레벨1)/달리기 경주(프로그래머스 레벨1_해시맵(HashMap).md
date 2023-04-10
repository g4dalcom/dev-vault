# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/178871)

## 📝 문제

얀에서는 매년 달리기 경주가 열립니다. 해설진들은 선수들이 자기 바로 앞의 선수를 추월할 때 추월한 선수의 이름을 부릅니다. 예를 들어 1등부터 3등까지 "mumu", "soe", "poe" 선수들이 순서대로 달리고 있을 때, 해설진이 "soe"선수를 불렀다면 2등인 "soe" 선수가 1등인 "mumu" 선수를 추월했다는 것입니다. 즉 "soe" 선수가 1등, "mumu" 선수가 2등으로 바뀝니다.

선수들의 이름이 1등부터 현재 등수 순서대로 담긴 문자열 배열 `players`와 해설진이 부른 이름을 담은 문자열 배열 `callings`가 매개변수로 주어질 때, 경주가 끝났을 때 선수들의 이름을 1등부터 등수 순서대로 배열에 담아 return 하는 solution 함수를 완성해주세요.

---

##### 제한사항

-   5 ≤ `players`의 길이 ≤ 50,000
    -   `players[i]`는 i번째 선수의 이름을 의미합니다.
    -   `players`의 원소들은 알파벳 소문자로만 이루어져 있습니다.
    -   `players`에는 중복된 값이 들어가 있지 않습니다.
    -   3 ≤ `players[i]`의 길이 ≤ 10
-   2 ≤ `callings`의 길이 ≤ 1,000,000
    -   `callings`는 `players`의 원소들로만 이루어져 있습니다.
    -   경주 진행중 1등인 선수의 이름은 불리지 않습니다.

---

##### 입출력 예

| players                               | callings                       | result |
|:------------------------------------- |:------------------------------ |:------ |
| ["mumu", "soe", "poe", "kai", "mine"] | ["kai", "kai", "mine", "mine"] | ["mumu", "kai", "mine", "soe", "poe"]       |


---

##### 입출력 예 설명

입출력 예 #1

4등인 "kai" 선수가 2번 추월하여 2등이 되고 앞서 3등, 2등인 "poe", "soe" 선수는 4등, 3등이 됩니다. 5등인 "mine" 선수가 2번 추월하여 4등, 3등인 "poe", "soe" 선수가 5등, 4등이 되고 경주가 끝납니다. 1등부터 배열에 담으면 ["mumu", "kai", "mine", "soe", "poe"]이 됩니다.

---

### 🔍 정답

```java
import java.util.*;

class Solution {
    public String[] solution(String[] players, String[] callings) {
        HashMap<String, Integer> map = new LinkedHashMap<>();
        HashMap<Integer, String> race = new LinkedHashMap<>();
        
        // 두 해시맵에 정보 담아주기
        for (int i = 0; i < players.length; i++) {
            map.put(players[i], i);
            race.put(i, players[i]);
        }
        
        /**
        * catchUp = 따라잡은 플레이어
        * caught = 따라잡힌 플레이어
        * idx = 따라잡은 플레이어의 앞 등수(따라잡힌 플레이어 등수)
        */
        for (int i = 0; i < callings.length; i++) {
            String catchUp = callings[i];
            int idx = map.get(catchUp)-1;
            
            String caught = race.get(idx);
            
            map.put(catchUp, idx);
            map.put(caught, idx+1);
            
            race.put(idx, catchUp);
            race.put(idx+1, caught);
        }
        
        String[] result = new String[players.length];
        for (int i = 0; i < players.length; i++) {
            result[i] = race.get(i);
        }
        
        return result;
    }
}
```
- HashMap<플레이어, 등수> 를 가지고는 따라잡은 플레이어의 등수는 구할 수 있지만(map.get(플레이어)) 따라잡힌 플레이어의 등수를 가져와서 갱신하려면 아래와 같이 map을 탐색해야 한다.

```java
// 호명된 플레이어의 앞 등수
int value = map.get(callings[i])-1;
int caught = "";

for (String player : map.keySet()) {
	if (map.get(player).equals(value)) {
		caught = player;
		break;
	}
}
```
- break를 걸어서 탐색을 최소화하여도 몇 테스트케이스에서 시간초과가 발생한다.
- 그래서 HashMap 두 개를 선언해서 각각 <플레이어, 등수> <등수, 플레이어> 로 놓고 두 가지를 모두 갱신하며 진행하였다.


### 🔎 정답

```java
import java.util.*;

class Solution {
    public String[] solution(String[] players, String[] callings) {
        HashMap<String, Integer> map = new LinkedHashMap<>();
        
        // 해시맵에 초깃값 담아주기
        for (int i = 0; i < players.length; i++) {
            map.put(players[i], i);
        }
        
        for (String race : callings) {
            int cur = map.get(race);            // 따라잡은 플레이어 현재 등수
            int prev = map.get(race) - 1;       // 앞 등수
            String caught = players[prev];      // 앞 플레이어
            
            // 순위 스위칭
            players[prev] = race;
            players[cur] = caught;
            
            map.put(race, prev);
            map.put(caught, cur);
        }
        
        String[] result = new String[players.length];
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            result[entry.getValue()] = entry.getKey();
        }
        
        return result;
    }
}
```
- 해시맵을 두 개 선언하지 않고 기존에 있던 players 배열을 그대로 이용하는 방법도 있다!