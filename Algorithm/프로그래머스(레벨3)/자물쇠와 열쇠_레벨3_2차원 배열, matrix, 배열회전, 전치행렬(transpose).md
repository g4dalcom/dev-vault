# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/60059)

## 📝 문제

고고학자인 **"튜브"**는 고대 유적지에서 보물과 유적이 가득할 것으로 추정되는 비밀의 문을 발견하였습니다. 그런데 문을 열려고 살펴보니 특이한 형태의 **자물쇠**로 잠겨 있었고 문 앞에는 특이한 형태의 **열쇠**와 함께 자물쇠를 푸는 방법에 대해 다음과 같이 설명해 주는 종이가 발견되었습니다.

잠겨있는 자물쇠는 격자 한 칸의 크기가 **`1 x 1`**인 **`N x N`** 크기의 정사각 격자 형태이고 특이한 모양의 열쇠는 **`M x M`** 크기인 정사각 격자 형태로 되어 있습니다.

자물쇠에는 홈이 파여 있고 열쇠 또한 홈과 돌기 부분이 있습니다. 열쇠는 회전과 이동이 가능하며 열쇠의 돌기 부분을 자물쇠의 홈 부분에 딱 맞게 채우면 자물쇠가 열리게 되는 구조입니다. 자물쇠 영역을 벗어난 부분에 있는 열쇠의 홈과 돌기는 자물쇠를 여는 데 영향을 주지 않지만, 자물쇠 영역 내에서는 열쇠의 돌기 부분과 자물쇠의 홈 부분이 정확히 일치해야 하며 열쇠의 돌기와 자물쇠의 돌기가 만나서는 안됩니다. 또한 자물쇠의 모든 홈을 채워 비어있는 곳이 없어야 자물쇠를 열 수 있습니다.

열쇠를 나타내는 2차원 배열 key와 자물쇠를 나타내는 2차원 배열 lock이 매개변수로 주어질 때, 열쇠로 자물쇠를 열수 있으면 true를, 열 수 없으면 false를 return 하도록 solution 함수를 완성해주세요.

### 제한사항

- key는 M x M(3 ≤ M ≤ 20, M은 자연수)크기 2차원 배열입니다.
- lock은 N x N(3 ≤ N ≤ 20, N은 자연수)크기 2차원 배열입니다.
- M은 항상 N 이하입니다.
- key와 lock의 원소는 0 또는 1로 이루어져 있습니다.
    - 0은 홈 부분, 1은 돌기 부분을 나타냅니다.

---

### 입출력 예

|key|lock|result|
|---|---|---|
|[[0, 0, 0], [1, 0, 0], [0, 1, 1]]|[[1, 1, 1], [1, 1, 0], [1, 0, 1]]|true|

### 입출력 예에 대한 설명

![자물쇠.jpg](https://grepp-programmers.s3.amazonaws.com/files/production/469703690b/79f2f473-5d13-47b9-96e0-a10e17b7d49a.jpg)

key를 시계 방향으로 90도 회전하고, 오른쪽으로 한 칸, 아래로 한 칸 이동하면 lock의 홈 부분을 정확히 모두 채울 수 있습니다.

---

### 💡 풀이

- 주어진 키로 자물쇠(Lock)의 홈을 모두 맞출 수 있는지 여부를 알아내야 하는 문제입니다. 주요 조건으로는 **키가 자물쇠 외부에도 위치할 수 있다**는 것과 **키를 90도씩 회전**시킬 수 있다는 것입니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlVZHZ%2FbtsCQq15YdJ%2FHoWGlxZ8tUsdkX1Qu7qBnK%2Fimg.png)

- 키와 자물쇠의 일치 여부는 배열을 확장시켜서 해결할 수 있습니다.
- 위와 같이 자물쇠 외부까지 설정한 배열을 탐색하는 것이죠! 이 때 각 행과 열의 길이는 **자물쇠의 길이**와 **키의 길이 * 2 - 2** 를 해주게 됩니다. 키가 왼쪽에서 시작해서 오른쪽 끝까지 갈 수 있어야 하므로 * 2 를 해주되 양 끝을 하나씩 포함하는 구조이기 때문입니다.

```java
int k = key.length;
int l = lock.length;
int size = l + k * 2 - 2;

map = new int[size][size];
```

- map 배열에서 자물쇠 외부는 탐색할 필요가 없으므로 자물쇠 구역인지를 확인하는 배열(boolean isLockSection)을 하나 더 선언하였고 자물쇠의 홈 부분을 모두 키의 돌기로 채워야 하기 때문에 자물쇠의 홈 개수(int lockCount) 도 미리 세어두었습니다.
- 그리고 map 의 첫 부분부터 자물쇠 부분만 탐색을 하는 것이죠!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Frp8H0%2FbtsCN1u3Hju%2Ft1sbZfUSW5L9B8AZc1DSv0%2Fimg.png)

- map의 해당 위치에서 매 탐색마다 탐색의 범위는 key의 크기(녹색 부분)이고 맵의 인덱스와 키의 인덱스를 대응시켜 주기 위해서 맵의 시작점을 넘겨주어서 탐색하였습니다.
- 위 그림을 예로 들면 시작 위치 row, col = (1, 1) 을 넘겨주어서 map의 1, 1과 key의 0, 0 을 시작으로 비교하는 것이죠!

```java
public boolean move(int[][] key, int[][] lock, int lockCount) {
        for (int i = 0; i < key.length + lock.length - 1; i++) {
            for (int j = 0; j < key.length + lock.length - 1; j++) {
                if (checking(key, i, j, lockCount)) return true;
            }
        }
        return false;
    }
    
    public boolean checking(int[][] key, int row, int col, int lockCount) {
        int count = lockCount;
        for (int i = 0; i < key.length; i++) {
            for (int j = 0; j < key[0].length; j++) {
                if (isLockSection[i+row][j+col]) {
                    if (map[i+row][j+col] == key[i][j]) return false;
                    else if (map[i+row][j+col] == 0 && key[i][j] == 1) {
                        count--;
                    }
                }
            }
        }  
        
        return count == 0;
    }
```

- 이 때 자물쇠 외부는 확인할 필요가 없기 때문에 lockSection = true 인 곳만, 그리고 홈과 홈이 만나거나 돌기와 돌기가 만난다면 false를 리턴하도록 하고 그 외 키의 돌기와 자물쇠의 홈이 자물쇠의 홈 개수만큼 만났을 때 true를 리턴하도록 하였습니다.
- 그리고 키는 90도씩 회전시킬 수 있기 때문에 이러한 탐색은 총 4번 이루어져야 하는데요! 이 알고리즘은 전치행렬을 이용해서 구현하였습니다.

![](https://blog.kakaocdn.net/dn/YmpuM/btsB7nZyupG/5xUvK0KHAJAZIJ6dnKMAFk/img.gif)

- 먼저 **전치행렬(transpose)** 을 만들고 양 끝 컬럼 교환들을 해주면 되는 것인데요!
- 전치행렬이란 어떠한 행렬의 열과 행을 바꾸어낸 행렬을 의미합니다.
- 예를 들어서 \[1, 2, 3\], \[4, 5, 6\] 을 전치하면, \[1, 4\], \[2, 5\], \[3, 6\] 으로 3 x 2 행렬이 2 x 3 이 되는 것이죠!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F0A5ui%2FbtsB7gTqsSV%2FiBccDMyv3QNkoGKVKjosQK%2Fimg.png)

- 만약 문제에서처럼 행과 열이 같다면 위와 같이 대각선을 기준으로 스왑이 이루어집니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    static int[][] map;
    static boolean[][] isLockSection;
    
    public boolean solution(int[][] key, int[][] lock) {
        int k = key.length;
        int l = lock.length;
        int size = l + k * 2 - 2;
        
        map = new int[size][size];
        isLockSection = new boolean[size][size];
        
        int rowStart = key.length - 1;
        int colStart = key[0].length - 1;
        int lockCount = 0;
        
        for (int i = 0; i < lock.length; i++) {
            for (int j = 0; j < lock[0].length; j++) {
                if (lock[i][j] == 0) lockCount++;
                map[rowStart+i][colStart+j] = lock[i][j];
                isLockSection[rowStart+i][colStart+j] = true;
            }
        }
        
        for (int i = 0; i < 4; i++) {
            if (move(key, lock,lockCount)) return true;
            rotate(key);
        }
        
        return false;
    }
    
    public void rotate(int[][] key) {
        for (int i = 0; i < key.length-1; i++) {
            for (int j = i+1; j < key.length; j++) {
                transpose(key, i, j);
            }
        }
        
        for (int i = 0; i < key.length; i++) {
            for (int j = 0; j < key.length/2; j++) {
                reverse(key, i, j, key.length-1-j);
            }
        }
    }
    
    public void transpose(int[][] key, int a, int b) {
        int temp = key[a][b];
        key[a][b] = key[b][a];
        key[b][a] = temp;
    }
    
    public void reverse(int[][] key, int i, int a, int b) {
        int temp = key[i][a];
        key[i][a] = key[i][b];
        key[i][b] = temp;
    }
    
    public boolean move(int[][] key, int[][] lock, int lockCount) {
        for (int i = 0; i < key.length + lock.length - 1; i++) {
            for (int j = 0; j < key.length + lock.length - 1; j++) {
                if (checking(key, i, j, lockCount)) return true;
            }
        }
        return false;
    }
    
    public boolean checking(int[][] key, int row, int col, int lockCount) {
        int count = lockCount;
        for (int i = 0; i < key.length; i++) {
            for (int j = 0; j < key[0].length; j++) {
                if (isLockSection[i+row][j+col]) {
                    if (map[i+row][j+col] == key[i][j]) return false;
                    else if (map[i+row][j+col] == 0 && key[i][j] == 1) {
                        count--;
                    }
                }
            }
        }  
        
        return count == 0;
    }
}
```