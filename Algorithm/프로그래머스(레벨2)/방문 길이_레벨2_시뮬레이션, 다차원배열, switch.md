# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/49994)

## 📝 문제

게임 캐릭터를 4가지 명령어를 통해 움직이려 합니다. 명령어는 다음과 같습니다.

- U: 위쪽으로 한 칸 가기
    
- D: 아래쪽으로 한 칸 가기
    
- R: 오른쪽으로 한 칸 가기
    
- L: 왼쪽으로 한 칸 가기
    

캐릭터는 좌표평면의 (0, 0) 위치에서 시작합니다. 좌표평면의 경계는 왼쪽 위(-5, 5), 왼쪽 아래(-5, -5), 오른쪽 위(5, 5), 오른쪽 아래(5, -5)로 이루어져 있습니다.

![방문길이1_qpp9l3.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/ace0e7bc-9092-4b95-9bfb-3a55a2aa780e/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B51_qpp9l3.png)

예를 들어, "ULURRDLLU"로 명령했다면

![방문길이2_lezmdo.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/668c7458-e184-472d-9d32-f5d2acca759a/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B52_lezmdo.png)

- 1번 명령어부터 7번 명령어까지 다음과 같이 움직입니다.

![방문길이3_sootjd.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/08558e36-d667-4160-bfec-b754c78a7d85/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B53_sootjd.png)

- 8번 명령어부터 9번 명령어까지 다음과 같이 움직입니다.

![방문길이4_hlpiej.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/a52af28e-5835-438b-9f40-5467ebf9bf03/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B54_hlpiej.png)

이때, 우리는 게임 캐릭터가 지나간 길 중 **캐릭터가 처음 걸어본 길의 길이**를 구하려고 합니다. 예를 들어 위의 예시에서 게임 캐릭터가 움직인 길이는 9이지만, 캐릭터가 처음 걸어본 길의 길이는 7이 됩니다. (8, 9번 명령어에서 움직인 길은 2, 3번 명령어에서 이미 거쳐 간 길입니다)

단, 좌표평면의 경계를 넘어가는 명령어는 무시합니다.

예를 들어, "LULLLLLLU"로 명령했다면

![방문길이5_nitjwj.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/f631f005-f8de-4392-a76c-a9ef64b6de08/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B55_nitjwj.png)

- 1번 명령어부터 6번 명령어대로 움직인 후, 7, 8번 명령어는 무시합니다. 다시 9번 명령어대로 움직입니다.

![방문길이6_nzhumd.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/35e62f0a-43c6-4142-bec6-6d28fbc57216/%E1%84%87%E1%85%A1%E1%86%BC%E1%84%86%E1%85%AE%E1%86%AB%E1%84%80%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%B56_nzhumd.png)

이때 캐릭터가 처음 걸어본 길의 길이는 7이 됩니다.

명령어가 매개변수 dirs로 주어질 때, 게임 캐릭터가 처음 걸어본 길의 길이를 구하여 return 하는 solution 함수를 완성해 주세요.

##### 제한사항

- dirs는 string형으로 주어지며, 'U', 'D', 'R', 'L' 이외에 문자는 주어지지 않습니다.
- dirs의 길이는 500 이하의 자연수입니다.

##### 입출력 예

|dirs|answer|
|---|---|
|"ULURRDLLU"|7|
|"LULLLLLLU"|7|

---

### 💡 풀이

- 이 문제에서 가장 유의깊게 보아야 하는 조건은 처음 도착한 좌표(지점)이 아니라 **처음 걸어본 길**이라는 것입니다.
- 좌표(지점)이라면 (1, 2) 라는 형식으로 표시할 수 있고 지나간 길이라면 visit\[1\]\[2\] = true 형식으로 표시할 수도 있겠죠!
- 그런데 **길은 점이 아니라 선**이기 때문에 방향이 존재합니다. 예를 들어서 (5, 5) 라는 좌표는 (4, 5)에서 올 수도 있고 (5, 6)에서 올 수 있습니다.
- 따라서 2차원 배열이 아니라 3차원 배열을 선언해서 방향도 저장해주도록 구현했습니다.
- 또 한 가지 주의할 점은, (5, 5) 에서 (4, 5)로 이동했다고 가정할 때 (4, 5) 좌표에 도착한 방향을 표시해주어야 할 뿐 아니라 (5, 5) 좌표의 시작한 방향 또한 표시해주어야 합니다. 
- 왜냐하면 어느 한쪽으로 이동했다면 반대 방향에서 오는 것도 같은 길을 이용한 것이기 때문입니다.
- map\[4\]\[5\]\[1\] = 1 이라고 표시했다면 반대로 map\[5\]\[5]\[2\] = 1 도 표시해주는 식입니다.
- 또한 마지막으로... 맵의 범위를 넘어가면 제자리에 있지만 이미 갔던 길이라면 카운팅을 하지 않되 이동은 해주어야 합니다!


### 🔍 정답

```java
class Solution {
	// 11의 길이와 1~4라는 방향을 가진 3차원 배열
    // U = 1, D = 2, R = 3, L = 4
    static int[][][] map = new int[11][11][5];
    static int firstStepCount = 0;
    
    public int solution(String dirs) {
	    // 시작 좌표를 (5, 5)로 정의
        int row = 5;
        int col = 5;

        for (int i = 0; i < dirs.length(); i++) {
            char command = dirs.charAt(i);
            Node newNode = new Node(row, col);
            movingNode(newNode, command);
            row = newNode.x;
            col = newNode.y;
        }

        return firstStepCount;
    }

    public void movingNode(Node node, char command) {
        switch (command) {
            case 'U':
                if (node.x-1 >= 0) {
                    if (map[node.x-1][node.y][1] == 0) {
                        firstStepCount++;
                    }
                    map[node.x-1][node.y][1] = 1;
                    map[node.x][node.y][2] = 1;
                    node.x -= 1;
                }        
                break;
            case 'D':
                if (node.x+1 <= 10) {
                    if (map[node.x+1][node.y][2] == 0) {
                        firstStepCount++;
                    }
                    map[node.x+1][node.y][2] = 1;
                    map[node.x][node.y][1] = 1;
                    node.x += 1;
                }
                
                break;
            case 'R':
                if (node.y+1 <= 10) {
                    if (map[node.x][node.y+1][3] == 0) {
                        firstStepCount++;
                    }
                    map[node.x][node.y+1][3] = 1;
                    map[node.x][node.y][4] = 1; 
                    node.y += 1;
                }
                break;
            case 'L':
                if (node.y-1 >= 0) {
                    if (map[node.x][node.y-1][4] == 0) {
                        firstStepCount++;
                    }
                    map[node.x][node.y-1][4] = 1;
                    map[node.x][node.y][3] = 1;
                    node.y -= 1;
                }       
                break;
        }
    }

    public class Node {
        int x, y;

        public Node(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
}
```