# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/87694)

## 📝 문제

다음과 같은 다각형 모양 지형에서 캐릭터가 아이템을 줍기 위해 이동하려 합니다.

![rect_1.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/9b96b07f-72db-4b1c-bd7a-6a9c9b8d0dc6/rect_1.png)

지형은 각 변이 x축, y축과 평행한 직사각형이 겹쳐진 형태로 표현하며, 캐릭터는 이 다각형의 둘레(굵은 선)를 따라서 이동합니다.

만약 직사각형을 겹친 후 다음과 같이 중앙에 빈 공간이 생기는 경우, 다각형의 가장 바깥쪽 테두리가 캐릭터의 이동 경로가 됩니다.

![rect_2.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/38b0739b-8dd8-40d8-ac44-c71678d28d07/rect_2.png)

단, 서로 다른 두 직사각형의 x축 좌표 또는 y축 좌표가 같은 경우는 없습니다.

![rect_4.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/ec976181-987e-494e-bb2d-0615ce16252f/rect_4.png)

즉, 위 그림처럼 서로 다른 두 직사각형이 꼭짓점에서 만나거나, 변이 겹치는 경우 등은 없습니다.

다음 그림과 같이 지형이 2개 이상으로 분리된 경우도 없습니다.

![rect_3.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/7eda8d92-ebe0-4b5f-bd15-0c9dc7af3a3e/rect_3.png)

한 직사각형이 다른 직사각형 안에 완전히 포함되는 경우 또한 없습니다.

![rect_7.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/1e178b0d-6580-4981-aae3-dd82a1b95362/rect_7.png)

지형을 나타내는 직사각형이 담긴 2차원 배열 rectangle, 초기 캐릭터의 위치 characterX, characterY, 아이템의 위치 itemX, itemY가 solution 함수의 매개변수로 주어질 때, 캐릭터가 아이템을 줍기 위해 이동해야 하는 가장 짧은 거리를 return 하도록 solution 함수를 완성해주세요.

##### 제한사항

- rectangle의 세로(행) 길이는 1 이상 4 이하입니다.
- rectangle의 원소는 각 직사각형의 [좌측 하단 x, 좌측 하단 y, 우측 상단 x, 우측 상단 y] 좌표 형태입니다.
    - 직사각형을 나타내는 모든 좌표값은 1 이상 50 이하인 자연수입니다.
    - 서로 다른 두 직사각형의 x축 좌표, 혹은 y축 좌표가 같은 경우는 없습니다.
    - 문제에 주어진 조건에 맞는 직사각형만 입력으로 주어집니다.
- charcterX, charcterY는 1 이상 50 이하인 자연수입니다.
    - 지형을 나타내는 다각형 테두리 위의 한 점이 주어집니다.
- itemX, itemY는 1 이상 50 이하인 자연수입니다.
    - 지형을 나타내는 다각형 테두리 위의 한 점이 주어집니다.
- 캐릭터와 아이템의 처음 위치가 같은 경우는 없습니다.

---

- 전체 배점의 50%는 직사각형이 1개인 경우입니다.  
    
- 전체 배점의 25%는 직사각형이 2개인 경우입니다.  
    
- 전체 배점의 25%는 직사각형이 3개 또는 4개인 경우입니다.  
    

---

##### 입출력 예

|rectangle|characterX|characterY|itemX|itemY|result|
|---|---|---|---|---|---|
|[[1,1,7,4],[3,2,5,5],[4,3,6,9],[2,6,8,8]]|1|3|7|8|17|
|[[1,1,8,4],[2,2,4,9],[3,6,9,8],[6,3,7,7]]|9|7|6|1|11|
|[[1,1,5,7]]|1|1|4|7|9|
|[[2,1,7,5],[6,4,10,10]]|3|1|7|10|15|
|[[2,2,5,5],[1,3,6,4],[3,1,4,6]]|1|4|6|3|10|

##### 입출력 예 설명

입출력 예 #1

![rect_5.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/7b89552b-f7b6-47e7-8bbd-deaf01907f70/rect_5.png)

캐릭터 위치는 (1, 3)이며, 아이템 위치는 (7, 8)입니다. 위 그림과 같이 굵은 선을 따라 이동하는 경로가 가장 짧습니다.

입출력 예 #2

![rect_6.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/ac6911d0-e386-472b-a109-2542214c8d6b/rect_6.png)

캐릭터 위치는 (9, 7)이며, 아이템 위치는 (6, 1)입니다. 위 그림과 같이 굵은 선을 따라 이동하는 경로가 가장 짧습니다.

입출력 예 #3

![rect_8.png](https://grepp-programmers.s3.ap-northeast-2.amazonaws.com/files/production/9c47ca5c-df4b-4b2e-8c5b-faf0815de665/rect_8.png)

캐릭터 위치는 (1, 1)이며, 아이템 위치는 (4, 7)입니다. 위 그림과 같이 굵은 선을 따라 이동하는 경로가 가장 짧습니다.

---

### 💡 풀이

- 최단거리를 구하는 BFS 문제의 약간 심화된 응용문제입니다!
- 우선 풀이를 위해 해결해야 하는 것은 **모서리를 어떻게 판별**할 것인가와 **겹쳐져 있는 구역을 어떻게 판별**할 것인지였습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fchi9iC%2Fbtsy9QKN157%2FEC18sC8EIvJkKoUmK3hRo0%2Fimg.png)

- (1, 1)과 (7, 4)를 꼭지점으로 갖는 사각형을 예로 들면 사각형의 크기 안에서 x가 7인 경우, x가 1인 경우, y가 1인 경우, y가 4인 경우에 각각 모서리임을 알 수 있습니다.

```java
for (int j = left_X; j <= right_X; j++) {
	for (int k = left_Y; k <= right_Y; k++) {
		if (j == left_X || j == right_X || k == left_Y || k == right_Y) {
			if (map[j][k] != 2) map[j][k] = 1;
		} else {
			map[j][k] = 2;
		}
	}
}
```

- 따라서 위와 같이 모서리를 판별하여 값을 넣어줄 수 있죠!
- 그리고 해당 사각형 안에서 그 외 좌표들은 사각형 내부가 되므로 내부에 해당하는 값(2)을 넣어줍니다.
- 그러면 어떤 사각형의 내부에 해당하는 좌표에 다른 사각형이 그려지게 되면 그 사각형도 내부에 속하는 식으로 그려지겠죠!
- 또한 새로운 사각형이 그려지면서 기존에 있던 모서리가 새로운 사각형의 내부가 된다면 그 모서리는 사각형의 내부(2)로 바뀌게 되는 식입니다.
- 기본적으로 map에 모서리 = 1, 내부 = 2, 지나간 좌표 = 3으로 초기화를 하면서 거리를 재면 되는데 또 다른 문제가 있었습니다 ㅠㅠ

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcvfwIU%2Fbtszde4OiIV%2FChnCdSjByqfDQLRvG7NEvK%2Fimg.png)

- 계속 헤매다가 다른 분의 힌트를 듣고 해결할 수 있었던 부분입니다...
- (1, 1)에서 (1, 2) 로 가는 길이 다이렉트로 뚫려있지 않고 (1, 1) > (1, 2) > (2, 2) > (1, 2) 로 가야 한다고 할 때, BFS 탐색을 하게 되면 (1, 1)도 모서리인 1이고 (1, 2)도 1이기 때문에 바로 갈 수 있다고 판단하게 되는 오류가 있었습니다.
- 좌표는 점이고 움직임은 선이며 선은 방향이 있기 때문에 생겨난 오류인데 이 문제는 전체적인 좌표를 2배 늘림으로써 해결할 수 있습니다.
- 위의 오른쪽 그림과 같이 한 칸 떨어져있는 좌표는 두 번의 움직임으로 도달할 수 있는데 이어져있지 않은 좌표라면 중간이 비어서 이동하지 않는 것이죠.
- 따라서 map의 크기와 캐릭터의 좌표, 아이템의 좌표를 모두 2배로 계산하고 마지막에 거리를 2로 나누게 되면 올바른 거리를 구할 수 있게 됩니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    static int[] dx = {0, 0, 1, -1};
    static int[] dy = {1, -1, 0, 0};
    static int[][] map;
    static int moveCount = 0;
    
    public int solution(int[][] rectangle, int characterX, int characterY, int itemX, int itemY) {
        map = new int[101][101];
        
        for (int i = 0; i < rectangle.length; i++) {
            int left_X = rectangle[i][0] * 2;
            int left_Y = rectangle[i][1] * 2;
            int right_X = rectangle[i][2] * 2;
            int right_Y = rectangle[i][3] * 2;
            
            for (int j = left_X; j <= right_X; j++) {
                for (int k = left_Y; k <= right_Y; k++) {
                    if (j == left_X || j == right_X || k == left_Y || k == right_Y) {
                        if (map[j][k] != 2) map[j][k] = 1;
                    } else {
                        map[j][k] = 2;
                    }
                }
            }
        }
        
        bfs(new Node(characterX * 2, characterY * 2, 0), itemX * 2, itemY * 2);
        
        return moveCount / 2;
    }
    
    public void bfs(Node node, int itemX, int itemY) {
        Queue<Node> q = new LinkedList<>();
        q.add(node);
        
        while (!q.isEmpty()) {
            Node current = q.poll();
            
            if (current.x == itemX && current.y == itemY) {
                moveCount = current.distance;
                break;
            }
            
            for (int i = 0; i < 4; i++) {
                int cx = current.x + dx[i];
                int cy = current.y + dy[i];
                
                if (cx >= 0 && cy >= 0 && cx < map.length && cy < map[0].length) {
                    if (map[cx][cy] == 1) {
                        q.add(new Node(cx, cy, current.distance + 1));
                        map[cx][cy] = 3;
                    }
                }
            }
        }
    }
    
    public class Node {
        int x, y, distance;
        
        public Node(int x, int y, int distance) {
            this.x = x;
            this.y = y;
            this.distance = distance;
        }
    }
}
```