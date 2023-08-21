# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/1829)

## 📝 문제

## 카카오 프렌즈 컬러링북

출판사의 편집자인 어피치는 네오에게 컬러링북에 들어갈 원화를 그려달라고 부탁하여 여러 장의 그림을 받았다. 여러 장의 그림을 난이도 순으로 컬러링북에 넣고 싶었던 어피치는 영역이 많으면 색칠하기가 까다로워 어려워진다는 사실을 발견하고 그림의 난이도를 영역의 수로 정의하였다. (영역이란 상하좌우로 연결된 같은 색상의 공간을 의미한다.)

그림에 몇 개의 영역이 있는지와 가장 큰 영역의 넓이는 얼마인지 계산하는 프로그램을 작성해보자.

![alt text](http://t1.kakaocdn.net/codefestival/apeach.png)

위의 그림은 총 12개 영역으로 이루어져 있으며, 가장 넓은 영역은 어피치의 얼굴면으로 넓이는 120이다.

### 입력 형식

입력은 그림의 크기를 나타내는 `m`과 `n`, 그리고 그림을 나타내는 `m × n` 크기의 2차원 배열 `picture`로 주어진다. 제한조건은 아래와 같다.

- `1 <= m, n <= 100`
- `picture`의 원소는 `0` 이상 `2^31 - 1` 이하의 임의의 값이다.
- `picture`의 원소 중 값이 `0`인 경우는 색칠하지 않는 영역을 뜻한다.

### 출력 형식

리턴 타입은 원소가 두 개인 정수 배열이다. 그림에 몇 개의 영역이 있는지와 가장 큰 영역은 몇 칸으로 이루어져 있는지를 리턴한다.

### 예제 입출력

|m|n|picture|answer|
|---|---|---|---|
|6|4|[[1, 1, 1, 0], [1, 2, 2, 0], [1, 0, 0, 1], [0, 0, 0, 1], [0, 0, 0, 3], [0, 0, 0, 3]]|[4, 5]|

### 예제에 대한 설명

예제로 주어진 그림은 총 4개의 영역으로 구성되어 있으며, 왼쪽 위의 영역과 오른쪽의 영역은 모두 `1`로 구성되어 있지만 상하좌우로 이어져있지 않으므로 다른 영역이다. 가장 넓은 영역은 왼쪽 위 `1`이 차지하는 영역으로 총 5칸이다.

---

### 💡 풀이

- 구해야 하는 값은 총 영역의 개수와 가장 큰 영역의 크기입니다. 이 두 가지 값은 각각 numberOfArea, maxSizeOfOneArea 라는 변수에 담을 예정입니다.
- 그리고 탐색 조건에 맞는 곳을 탐색하게 되면 visit = true로 바꿀 것입니다.

1. 2차원 배열을 순차적으로 탐색합니다.
	- visit = false 이면서 해당 좌표의 value가 0보다 큰 경우만 탐색합니다.
	- visit = true라는 것은 이미 다른 영역에 포함되는 좌표라는 뜻이고 좌표의 value가 0이라는 것은 색칠되지 않은 영역이므로 제외해야 합니다.
2. 탐색할 좌표가 발견되었다면 numberOfArea++을 해서 영역의 개수를 카운팅해주고 해당 좌표의 value값을 함께 넘겨주어서 bfs 탐색을 합니다.
	- 이미 탐색한 영역과 색칠되지 않은 영역을 빼고 새로운 영역이 발견될 때마다 numberOfArea 값을 카운팅하므로 결국 영역의 개수가 구해지는 것입니다!
3. 현재 좌표값을 기준으로 위, 아래, 좌, 우를 탐색합니다.
	- 전체 영역의 크기를 벗어나지 않으면서 파라미터로 넘겨받은 value값과 일치하는 곳이 발견되면 areaCount++ 을 하고 발견된 좌표를 새롭게 큐에 넣습니다.
	- 큐가 빌 때까지 반복합니다.
	- areaCount는 해당 영역이 얼마나 큰 영역인지를 구하는 것입니다.

### 🔍 정답

```java
import java.util.*;

class Solution {    
    static int[] dx = {0, 0, 1, -1};
    static int[] dy = {1, -1, 0, 0};
    static boolean[][] visit;
    
    public int[] solution(int m, int n, int[][] picture) {
        int numberOfArea = 0;
        int maxSizeOfOneArea = 0;
        visit = new boolean[m][n];
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (!visit[i][j] && picture[i][j] > 0) {
                    numberOfArea++;
                    maxSizeOfOneArea = Math.max(maxSizeOfOneArea, bfs(picture[i][j], i, j, m, n, picture));
                }
            }
        }
        return new int[] {numberOfArea, maxSizeOfOneArea};
    }
    
    public int bfs(int baseNumber, int row, int col, int m, int n, int[][] picture) {
        Queue<Location> q = new LinkedList<>();
        q.add(new Location(row, col));
        visit[row][col] = true;
        
        int areaCount = 1;
        while (!q.isEmpty()) {
            Location lo = q.poll();
            
            for (int i = 0; i < 4; i++) {
                int cx = lo.x + dx[i];
                int cy = lo.y + dy[i];
                
                if (cx >= 0 && cy >= 0 && cx < m && cy < n) {
                    if (!visit[cx][cy] && picture[cx][cy] == baseNumber) {
                        visit[cx][cy] = true;
                        q.add(new Location(cx, cy));
                        areaCount++;
                    }
                }
            }
        }
        return areaCount;
    }
    
    public class Location {
        int x, y;
        
        public Location(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
}
```