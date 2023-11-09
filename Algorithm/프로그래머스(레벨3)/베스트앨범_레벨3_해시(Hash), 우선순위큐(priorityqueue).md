# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42579)

## 📝 문제

스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.

1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.

##### 제한사항

- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.

##### 입출력 예

|genres|plays|return|
|---|---|---|
|["classic", "pop", "classic", "classic", "pop"]|[500, 600, 150, 800, 2500]|[4, 1, 3, 0]|

##### 입출력 예 설명

classic 장르는 1,450회 재생되었으며, classic 노래는 다음과 같습니다.

- 고유 번호 3: 800회 재생
- 고유 번호 0: 500회 재생
- 고유 번호 2: 150회 재생

pop 장르는 3,100회 재생되었으며, pop 노래는 다음과 같습니다.

- 고유 번호 4: 2,500회 재생
- 고유 번호 1: 600회 재생

따라서 pop 장르의 [4, 1]번 노래를 먼저, classic 장르의 [3, 0]번 노래를 그다음에 수록합니다.

- 장르 별로 가장 많이 재생된 노래를 최대 두 개까지 모아 베스트 앨범을 출시하므로 2번 노래는 수록되지 않습니다.

---

### 💡 풀이

- 문제의 분류는 **해시**라고 되어있는데 주 알고리즘은 정렬이었던 것 같습니다.
- 문제의 조건을 하나씩 해결해보면,
- 첫 번째로 **가장 많이 재생된 장르**를 먼저 수록해야 합니다. 따라서 장르별 재생횟수를 따져보아야 하는데 해시맵의 **getOrDeafault** 메소드를 이용해서 횟수를 누적하여 저장하였습니다. 그리고 해시맵 요소들을 다시 우선순위큐(genrePQ)에 넣으며 횟수를 기준으로 내림차순 정렬하였습니다.
- 두 번째로 **장르내에서 많이 재생된 노래를 최대 2개 수록**해야 합니다. 여기서 재생횟수가 같다면 **낮은 고유번호순**으로 수록합니다. 그러면 해당 노래의 **고유번호**와 **재생횟수**를 알아야겠네요! 이것 또한 정렬을 위해 우선순위큐(songPQ)에 저장할 것입니다.
- 두 개의 우선순위 큐가 존재하는데, 먼저 genrePQ는 많이 재생된 장르가 우선순위가 되어 추출됩니다.
- 추출된 장르와 일치하는 고유번호와 재생횟수를 songPQ에 저장해주는 것입니다. 그리고 최대 2개만 꺼내서 result 배열에 저장하면 문제의 조건을 달성할 수가 있죠!
- 이 문제는 stream만을 이용해서 풀이한 굇수도 있고 해시맵을 2개 사용한 풀이 등 정말 다양한 풀이가 있었습니다. 풀이 과정이 어렵진 않았는데 풀고나서 다른 사람들의 풀이를 공부하는 게 재밌었던 문제였네요!

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int[] solution(String[] genres, int[] plays) {
        ArrayList<Integer> result = new ArrayList<>();
        HashMap<String, Integer> map = new HashMap<>();
        PriorityQueue<Genre> genrePQ = new PriorityQueue<>((o1, o2) -> o2.count - o1.count);
        PriorityQueue<Song> songPQ = new PriorityQueue<>((o1, o2) -> o1.playCount == o2.playCount ? o1.id - o2.id : o2.playCount - o1.playCount);
        
        // 해시맵에 장르와 총 플레이 횟수 저장
        for (int i = 0; i < genres.length; i++) {
            map.put(genres[i], map.getOrDefault(genres[i], 0) + plays[i]);
        }
        
        // 총 플레이 횟수가 많은 순서로 꺼내기 위해 우선순위큐에 장르 저장
        for (String key : map.keySet()) {
            genrePQ.add(new Genre(key, map.get(key)));
        }
        
        // 플레이 횟수 많은 순서로 장르 꺼내기
        while (!genrePQ.isEmpty()) {
            String genre = genrePQ.poll().genre;
            int count = 2;
            
            // 해당 장르의 고유번호와 플레이횟수를 우선순위큐에 저장
            for (int i = 0; i < genres.length; i++) {
                if (genres[i].equals(genre)) {
                    songPQ.add(new Song(i, plays[i]));
                }
            }
            
            // 2개까지만 꺼내기
            while (!songPQ.isEmpty()) {
                int current = songPQ.poll().id;
                if (count-- > 0) {
                    result.add(current);
                }
            }
        }
        
        return result.stream().mapToInt(Integer::intValue).toArray();
    }
    
    public class Genre {
        String genre;
        int count;
        
        public Genre (String genre, int count) {
            this.genre = genre;
            this.count = count;
        }
    }
    
    public class Song {
        int id;
        int playCount;
        
        public Song(int id, int playCount) {
            this.id = id;
            this.playCount = playCount;
        }
    }
}
```