# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/172928)

## 📝 문제

지나다니는 길을 'O', 장애물을 'X'로 나타낸 직사각형 격자 모양의 공원에서 로봇 강아지가 산책을 하려합니다. 산책은 로봇 강아지에 미리 입력된 명령에 따라 진행하며, 명령은 다음과 같은 형식으로 주어집니다.

-   ["방향 거리", "방향 거리" … ]

예를 들어 "E 5"는 로봇 강아지가 현재 위치에서 동쪽으로 5칸 이동했다는 의미입니다. 로봇 강아지는 명령을 수행하기 전에 다음 두 가지를 먼저 확인합니다.

-   주어진 방향으로 이동할 때 공원을 벗어나는지 확인합니다.
-   주어진 방향으로 이동 중 장애물을 만나는지 확인합니다.

위 두 가지중 어느 하나라도 해당된다면, 로봇 강아지는 해당 명령을 무시하고 다음 명령을 수행합니다.  
공원의 가로 길이가 W, 세로 길이가 H라고 할 때, 공원의 좌측 상단의 좌표는 (0, 0), 우측 하단의 좌표는 (H - 1, W - 1) 입니다.

![image](https://user-images.githubusercontent.com/62426665/217702316-1bd5d3ba-c1d7-4133-bfb5-36bdc85a08ba.png)

공원을 나타내는 문자열 배열 `park`, 로봇 강아지가 수행할 명령이 담긴 문자열 배열 `routes`가 매개변수로 주어질 때, 로봇 강아지가 모든 명령을 수행 후 놓인 위치를 [세로 방향 좌표, 가로 방향 좌표] 순으로 배열에 담아 return 하도록 solution 함수를 완성해주세요.

---

##### 제한사항

-   3 ≤ `park`의 길이 ≤ 50
    -   3 ≤ `park[i]`의 길이 ≤ 50
        -   `park[i]`는 다음 문자들로 이루어져 있으며 시작지점은 하나만 주어집니다.
            -   S : 시작 지점
            -   O : 이동 가능한 통로
            -   X : 장애물
    -   `park`는 직사각형 모양입니다.
-   1 ≤ `routes`의 길이 ≤ 50
    -   `routes`의 각 원소는 로봇 강아지가 수행할 명령어를 나타냅니다.
    -   로봇 강아지는 `routes`의 첫 번째 원소부터 순서대로 명령을 수행합니다.
    -   `routes`의 원소는 "op n"과 같은 구조로 이루어져 있으며, op는 이동할 방향, n은 이동할 칸의 수를 의미합니다.
        -   op는 다음 네 가지중 하나로 이루어져 있습니다.
            -   N : 북쪽으로 주어진 칸만큼 이동합니다.
            -   S : 남쪽으로 주어진 칸만큼 이동합니다.
            -   W : 서쪽으로 주어진 칸만큼 이동합니다.
            -   E : 동쪽으로 주어진 칸만큼 이동합니다.
        -   1 ≤ n ≤ 9

---

##### 입출력 예

| park                      | routes              | result |
|:------------------------- |:------------------- |:------ |
| ["SOO","OOO","OOO"]       | ["E 2","S 2","W 1"] | [2,1]  |
| ["SOO","OXX","OOO"]       | ["E 2","S 2","W 1"] | [0,1] |
| ["OSO","OOO","OXO","OOO"] | ["E 2","S 3","W 1"] | [0,0]       |


---

##### 입출력 예 설명

입출력 예 #1

입력된 명령대로 동쪽으로 2칸, 남쪽으로 2칸, 서쪽으로 1칸 이동하면 [0,0] -> [0,2] -> [2,2] -> [2,1]이 됩니다.

입출력 예 #2

입력된 명령대로라면 동쪽으로 2칸, 남쪽으로 2칸, 서쪽으로 1칸 이동해야하지만 남쪽으로 2칸 이동할 때 장애물이 있는 칸을 지나기 때문에 해당 명령을 제외한 명령들만 따릅니다. 결과적으로는 [0,0] -> [0,2] -> [0,1]이 됩니다.

입출력 예 #3

처음 입력된 명령은 공원을 나가게 되고 두 번째로 입력된 명령 또한 장애물을 지나가게 되므로 두 입력은 제외한 세 번째 명령만 따르므로 결과는 다음과 같습니다. [0,1] -> [0,0]

---

### 🔍 정답

```java
import java.util.*;

class Solution {
    
    static int[][] map;
    static Queue<Command> q;
    static int x = 0;
    static int y = 0;
    static int dir[] = {-1, 1, -1, 1};
    static int nextX = 0;
    static int nextY = 0;
    
    public int[] solution(String[] park, String[] routes) {
        map = new int[park.length][park[0].length()];

        // 2차원 배열에 맵 그리고 시작 위치로 x, y값 초기화
        for (int i = 0; i < park.length; i++) {
            char[] ch = park[i].toCharArray();

			// 시작위치 0, 장애물 -1, 그 외 1
            for (int j = 0; j < park[0].length(); j++) {
                if (ch[j] == 'O') map[i][j] = 1;
                else if (ch[j] == 'X') map[i][j] = -1;
                else {
                    map[i][j] = 0;
                    x = i;
                    y = j;
                }
            }
        }

        // q에 명령어 넣어주기
        q = new LinkedList<>();
        for (int i = 0; i < routes.length; i++) {
            String[] str = routes[i].split(" ");
            q.offer(new Command(str[0], Integer.parseInt(str[1])));
        }

        // 명령어 실행
        while (!q.isEmpty()) {
            walk(park);
        }

        // 출력
        int[] result = new int[2];
        result[0] = x;
        result[1] = y;
        
        return result;
    }

    public static void walk(String[] park) {

        nextX = x;
        nextY = y;

        Command co = q.poll();

        for (int i = 0; i < co.distance; i++) {
            switch (co.direction) {
                case "N":
                    nextX += dir[0];
                    break;

                case "S":
                    nextX += dir[1];
                    break;

                case "W":
                    nextY += dir[2];
                    break;

                case "E":
                    nextY += dir[3];
                    break;
            }

            // 실행 도중 맵을 벗어나거나 장애물을 만나면 초깃값으로 돌리고 실행이 완료되면 새로운 위치값으로 초기화
            if (nextX < 0 || nextY < 0 || nextX >= park.length || nextY >= park[0].length() || map[nextX][nextY] == -1) {
                nextX = x;
                nextY = y;
                return;
            }
        }
        x = nextX;
        y = nextY;
    }

    public static class Command {
        String direction;
        int distance;

        public Command(String direction, int distance) {
            this.direction = direction;
            this.distance = distance;
        }
    }
}
```
- 공원의 위치는 String 배열로 주어지는데, 위치 파악하기가 까다로워서 2차원 int 배열을 선언한 후에 새롭게 넣어주었다.
- 그리고 명령어를 큐에 넣어준 후에 큐가 빌 때까지 명령어를 수행하도록 하였다.
- 중간에 장애물이 있는지 여부를 파악해야 해서 한 칸씩 이동하면서 확인하도록 했는데, 출발 위치와 도착 위치만 계산해놓고 그 사이에 X가 있는지 여부를 확인하는 방법도 가능할 거 같긴 하다!
- 이동하다가 중간에 조건을 벗어나는 경우에는 처음부터 이동하지 않은 것처럼 계산하기 위해서 초깃값을 주고 온전하게 실행이 완료되면 초깃값을 현재 위치값으로 다시 초기화하는 식으로 했는데 좀 더 깔끔한 방법을 써보고 싶어서 boolean이나 다른 여타 방법을 고민해봤는데 딱히 떠오르지는 않았다 ㅠㅠ
```java
            // 실행 도중 맵을 벗어나거나 장애물을 만나면 초깃값으로 돌리고 실행이 완료되면 새로운 위치값으로 초기화
            if (nextX < 0 || nextY < 0 || nextX >= park.length || nextY >= park[0].length() || map[nextX][nextY] == -1) {
                nextX = x;
                nextY = y;
                return;
            }
        }
        x = nextX;
        y = nextY;
``` 


### 💡 다른 사람 풀이

```java
class Solution {
    public static int[] solution(String[] park, String[] routes) {
        int x = 0;
        int y = 0;
        int[][] arr = {{-1,0},{1,0},{0,-1},{0,1}};
        boolean start = false;
        for(int i=0; i< park.length;i++){
            if(start){
                break;
            }
            for(int j=0; j<park[i].length();j++){
                if(park[i].charAt(j) == 'S'){
                    x = i;
                    y = j;
                    start = true;
                    break;
                }
            }
        }

        for (String route : routes) {
            String[] routeArr = route.split(" ");
            String direction = routeArr[0];
            int distance = Integer.parseInt(routeArr[1]);

            int index = getDirectionIndex(direction);

            if(isWalk(park, x, y, distance, arr[index])){
                x += distance*arr[index][0];
                y += distance*arr[index][1];
            }
        }

        return new int[]{x,y};
    }

    private static boolean isWalk(String[] park, int x, int y, int distance, int[] arr) {
        for(int i = 0; i< distance; i++){
            x+= arr[0];
            y+= arr[1];

            if(y<0 || y> park[0].length()-1 || x<0 || x> park.length-1 || park[x].charAt(y)=='X'){
                return false;
            }
        }
        return true;
    }

    private static int getDirectionIndex(String direction) {
        int index = 0;
        switch (direction){
            case "N":
                break;
            case "S":
                index = 1;
                break;
            case "W":
                index = 2;
                break;
            case "E":
                index = 3;
                break;
        }
        return index;
    }
}
```
- 나와 기본적인 풀이 방법은 비슷한데, 주어진 배열들만 가지고 해결을 했고 이동하는 메서드의 반환값을 boolean으로 해서 true일 때만 값을 갱신하는 방법을 사용하였다. 