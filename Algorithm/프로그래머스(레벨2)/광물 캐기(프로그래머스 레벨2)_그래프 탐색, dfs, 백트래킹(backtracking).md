# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/172927)

## 📝 문제

마인은 곡괭이로 광산에서 광석을 캐려고 합니다. 마인은 다이아몬드 곡괭이, 철 곡괭이, 돌 곡괭이를 각각 0개에서 5개까지 가지고 있으며, 곡괭이로 광물을 캘 때는 피로도가 소모됩니다. 각 곡괭이로 광물을 캘 때의 피로도는 아래 표와 같습니다.

![image](https://user-images.githubusercontent.com/62426665/217975815-63c58d04-0421-4c39-85ce-17613b9c9389.png)

예를 들어, 철 곡괭이는 다이아몬드를 캘 때 피로도 5가 소모되며, 철과 돌을 캘때는 피로도가 1씩 소모됩니다. 각 곡괭이는 종류에 상관없이 광물 5개를 캔 후에는 더 이상 사용할 수 없습니다.

마인은 다음과 같은 규칙을 지키면서 최소한의 피로도로 광물을 캐려고 합니다.

-   사용할 수 있는 곡괭이중 아무거나 하나를 선택해 광물을 캡니다.
-   한 번 사용하기 시작한 곡괭이는 사용할 수 없을 때까지 사용합니다.
-   광물은 주어진 순서대로만 캘 수 있습니다.
-   광산에 있는 모든 광물을 캐거나, 더 사용할 곡괭이가 없을 때까지 광물을 캡니다.

즉, 곡괭이를 하나 선택해서 광물 5개를 연속으로 캐고, 다음 곡괭이를 선택해서 광물 5개를 연속으로 캐는 과정을 반복하며, 더 사용할 곡괭이가 없거나 광산에 있는 모든 광물을 캘 때까지 과정을 반복하면 됩니다.

마인이 갖고 있는 곡괭이의 개수를 나타내는 정수 배열 `picks`와 광물들의 순서를 나타내는 문자열 배열 `minerals`가 매개변수로 주어질 때, 마인이 작업을 끝내기까지 필요한 최소한의 피로도를 return 하는 solution 함수를 완성해주세요.

---

##### 제한사항

-   `picks`는 [dia, iron, stone]과 같은 구조로 이루어져 있습니다.
    -   0 ≤ dia, iron, stone ≤ 5
    -   dia는 다이아몬드 곡괭이의 수를 의미합니다.
    -   iron은 철 곡괭이의 수를 의미합니다.
    -   stone은 돌 곡괭이의 수를 의미합니다.
    -   곡괭이는 최소 1개 이상 가지고 있습니다.
-   5 ≤ `minerals`의 길이 ≤ 50
    -   `minerals`는 다음 3개의 문자열로 이루어져 있으며 각각의 의미는 다음과 같습니다.
    -   diamond : 다이아몬드
    -   iron : 철
    -   stone : 돌

---

##### 입출력 예

| picks     | minerals                                                                                                   | result |
|:--------- |:---------------------------------------------------------------------------------------------------------- |:------ |
| [1, 3, 2] | ["diamond", "diamond", "diamond", "iron", "iron", "diamond", "iron", "stone"]                              | 12     |
| [0, 1, 1] | ["diamond", "diamond", "diamond", "diamond", "diamond", "iron", "iron", "iron", "iron", "iron", "diamond"] | 50       |


---

##### 입출력 예 설명

입출력 예 #1

다이아몬드 곡괭이로 앞에 다섯 광물을 캐고 철 곡괭이로 남은 다이아몬드, 철, 돌을 1개씩 캐면 12(1 + 1 + 1 + 1+ 1 + 5 + 1 + 1)의 피로도로 캘 수 있으며 이때가 최소값입니다.

입출력 예 #2

철 곡괭이로 다이아몬드 5개를 캐고 돌 곡괭이고 철 5개를 캐면 50의 피로도로 캘 수 있으며, 이때가 최소값입니다.

---

### 🔍 정답

```java
import java.util.*;

class Solution {
    
    static int depth;
    static int min = Integer.MAX_VALUE;
    static int calc = 0;
    static boolean[] visit;
    
    public int solution(int[] picks, String[] minerals) {
        
        /**
        * pickList = 곡괭이를 담은 리스트
        * 다이아: 0, 철: 1, 돌: 2
        * 개수별로 리스트에 중복되어 들어간다.
        * ex) 다이아 1, 철 3, 돌 1 -> pickList = [0, 1, 1, 1, 2];
        */
        int ea = 0;
        ArrayList<Integer> pickList = new ArrayList<>();
        for (int i = 0; i < picks.length; i++) {
            for (int j = 0; j < picks[i]; j++) {
                if (picks[i] > 0) {
                    pickList.add(i);
                    ea++;
                }
            }
        }
        
        /**
        * 종료조건
        * 광물을 5개씩 묶은 묶음의 개수 or 곡괭이의 개수
        * 광물을 모두 캐거나 곡괭이가 모두 소진되면 종료하기 위함
        */
        depth = Math.min((int) Math.ceil((double) minerals.length / 5), ea);
  
        visit = new boolean[pickList.size()];        
        dfs(0, pickList, minerals);
        
        return min;
    }

    public static void dfs(int idx, ArrayList<Integer> pickList, String[] minerals) {

        // 종료 조건, 계산 도중 min 이상이 되면 더 계산할 필요가 없으므로 리턴!
        if (calc >= min) return;
        
        // 종료 조건, 최솟값을 계산하고 리턴!
        if (idx == depth) {
            min = Math.min(min, calc);
            return;
        }

        // 탐색!
        for (int i = 0; i < pickList.size(); i++) {
            if (!visit[i]) {
                int tmp = fatigue(pickList.get(i), idx, minerals);
                visit[i] = true;
                calc += tmp;
                dfs(idx+1, pickList, minerals);
                visit[i] = false;
                calc -= tmp;
            }
        }
    }

    public static int fatigue(int num, int st, String[] minerals) {
        /**
        * st = 시작 인덱스
        * range = 시작 인덱스 + 5 or 광물 마지막 인덱스
        */
        st = st * 5;
        int range = Math.min(st+5, minerals.length);
        int sum = 0;

        for (int i = st; i < range; i++) {
            switch (minerals[i]) {
                case "diamond":
                    sum += num == 0 ? 1 : num == 1 ? 5 : 25;
                    break;

                case "iron":
                    sum += num == 0 ? 1 : num == 1 ? 1 : 5;
                    break;

                case "stone":
                    sum += 1;
                    break;
            }
        }

        return sum;
    }
}
```
- 그냥 백트래킹 + 순열로 풀 수 있겠다 싶어서 접근을 했었는데 곡괭이를 소진하면서 탐색하는 게 생각보다 까다로웠다. 그래서 좀 무식하게 리스트를 만들어서 곡괭이를 풀어헤친 후에 탐색을 했다.
	- picks = [1, 3, 2] -> pickList = [0, 1, 1, 1, 2, 2]
- 종료조건은 곡괭이를 모두 소진했을 때와 광물을 모두 캤을 때 두 가지 인데,
	- 광물은 한 번에 다섯개씩 캐야 하므로 5개가 한 묶음. 그러므로 광물의 총 개수 / 5 를 하면 곡괭이 하나 당 광물의 묶음이 나온다. 그런데 만약 광물이 딱 나누어떨어지지 않는 경우, 그러니까 광물의 총 개수가 5개 미만인 경우에도 곡괭이를 소진해야 하므로 올림 처리를 해서 묶음의 개수를 구한다.
	- 곡괭이를 모두 소진했을 경우를 계산하기 위해 곡괭이를 풀어헤치며 개수도 구하였다.

```java
depth = Math.min((int) Math.ceil((double) minerals.length / 5), ea);
```

- 피로도(fatigue)는 광물 묶음이 아니라 광물 각각으로 구해야하므로 광물 묶음의 첫번째 인덱스부터 마지막 인덱스까지 탐색을 해야 한다.
- 만약 첫 번째 광물 묶음이라면 0번 인덱스부터 4번 인덱스, 두 번째 광물 묶음이라면 5번 인덱스부터 9번 인덱스까지 5개씩 탐색을 해야 하므로,
	- 현재 광물 묶음의 번호(st) * 5 를 시작 인덱스로 설정하였다.

```java
    public static int fatigue(int num, int st, String[] minerals) {
        /**
        * st = 시작 인덱스
        * range = 시작 인덱스 + 5 or 광물 마지막 인덱스
        */
        st = st * 5;
        int range = Math.min(st+5, minerals.length);
        int sum = 0;
```

- 그리고 종료 조건을 설정해놓고 백트래킹을 하면 되는데
```java
// 종료 조건, 최솟값을 계산하고 리턴!
if (idx == depth) {
	min = Math.min(min, calc);
	return;
}
```

- 이렇게만 하면 테스트케이스 25, 34, 35번이 시간 초과가 된다. 그러면 탐색을 줄일 방법을 찾아보아야 한다.

```java
if (calc >= min) return;
```
- 탐색을 하다가 계산값이 min값 이상이 되면 더 계산할 필요가 없어지므로 이 때에도 리턴하도록 조건을 추가하면 통과된다!