# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/17683)

## 📝 문제

라디오를 자주 듣는 네오는 라디오에서 방금 나왔던 음악이 무슨 음악인지 궁금해질 때가 많다. 그럴 때 네오는 다음 포털의 '방금그곡' 서비스를 이용하곤 한다. 방금그곡에서는 TV, 라디오 등에서 나온 음악에 관해 제목 등의 정보를 제공하는 서비스이다.

네오는 자신이 기억한 멜로디를 가지고 방금그곡을 이용해 음악을 찾는다. 그런데 라디오 방송에서는 한 음악을 반복해서 재생할 때도 있어서 네오가 기억하고 있는 멜로디는 음악 끝부분과 처음 부분이 이어서 재생된 멜로디일 수도 있다. 반대로, 한 음악을 중간에 끊을 경우 원본 음악에는 네오가 기억한 멜로디가 들어있다 해도 그 곡이 네오가 들은 곡이 아닐 수도 있다. 그렇기 때문에 네오는 기억한 멜로디를 재생 시간과 제공된 악보를 직접 보면서 비교하려고 한다. 다음과 같은 가정을 할 때 네오가 찾으려는 음악의 제목을 구하여라.

- 방금그곡 서비스에서는 음악 제목, 재생이 시작되고 끝난 시각, 악보를 제공한다.
- 네오가 기억한 멜로디와 악보에 사용되는 음은 C, C#, D, D#, E, F, F#, G, G#, A, A#, B 12개이다.
- 각 음은 1분에 1개씩 재생된다. 음악은 반드시 처음부터 재생되며 음악 길이보다 재생된 시간이 길 때는 음악이 끊김 없이 처음부터 반복해서 재생된다. 음악 길이보다 재생된 시간이 짧을 때는 처음부터 재생 시간만큼만 재생된다.
- 음악이 00:00를 넘겨서까지 재생되는 일은 없다.
- 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환한다. 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환한다.
- 조건이 일치하는 음악이 없을 때에는 “`(None)`”을 반환한다.

### 입력 형식

입력으로 네오가 기억한 멜로디를 담은 문자열 `m`과 방송된 곡의 정보를 담고 있는 배열 `musicinfos`가 주어진다.

- `m`은 음 `1`개 이상 `1439`개 이하로 구성되어 있다.
- `musicinfos`는 `100`개 이하의 곡 정보를 담고 있는 배열로, 각각의 곡 정보는 음악이 시작한 시각, 끝난 시각, 음악 제목, 악보 정보가 '`,`'로 구분된 문자열이다.
    - 음악의 시작 시각과 끝난 시각은 24시간 `HH:MM` 형식이다.
    - 음악 제목은 '`,`' 이외의 출력 가능한 문자로 표현된 길이 `1` 이상 `64` 이하의 문자열이다.
    - 악보 정보는 음 `1`개 이상 `1439`개 이하로 구성되어 있다.

### 출력 형식

조건과 일치하는 음악 제목을 출력한다.

### 입출력 예시

|m|musicinfos|answer|
|---|---|---|
|"ABCDEFG"|["12:00,12:14,HELLO,CDEFGAB", "13:00,13:05,WORLD,ABCDEF"]|"HELLO"|
|"CC#BCC#BCC#BCC#B"|["03:00,03:30,FOO,CC#B", "04:00,04:08,BAR,CC#BCC#BCC#B"]|"FOO"|
|"ABC"|["12:00,12:14,HELLO,C#DEFGAB", "13:00,13:05,WORLD,ABCDEF"]|"WORLD"|

### 설명

첫 번째 예시에서 HELLO는 길이가 7분이지만 12:00부터 12:14까지 재생되었으므로 실제로 CDEFGABCDEFGAB로 재생되었고, 이 중에 기억한 멜로디인 ABCDEFG가 들어있다.  
세 번째 예시에서 HELLO는 C#DEFGABC#DEFGAB로, WORLD는 ABCDE로 재생되었다. HELLO 안에 있는 ABC#은 기억한 멜로디인 ABC와 일치하지 않고, WORLD 안에 있는 ABC가 기억한 멜로디와 일치한다.

---

### 💡 풀이

- 처음에는 문제를 이해하는 데도 시간이 걸렸습니다. 
- 배열 각 요소를 "," 를 기준으로 split 했을 때, 3번 인덱스인 멜로디가 **1번 인덱스 - 0번 인덱스** 를 한 시간만큼 재생이 되는 것입니다.
- 문제에서 고민을 해야되는 부분은 아래와 같습니다.
	- 시간 차이를 어떻게 계산할 것인가
	- ABC인 노래가 연속해서 재생된다면 ABCABCABC 이런식이 될 것인데 이것을 어떻게 구현할 것인가
	- 음에 **#** 이 들어가는 것은 어떻게 처리할 것인가
	- 음악이 입력된 멜로디를 포함하는지 어떻게 확인할 것인가(\#\ 처리를 포함해서)
	- 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환하고 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환해야 하는 조건을 어떻게 구현할 것인가
- 뭔가 이것저것 고민해야 될 부분이 매우 많군요.... 아니나다를까 풀릴듯 풀릴듯 꽤 오랜 시간을 잡아먹었습니다 ㅠㅠ
- 어쨌든 구현한 것들을 하나씩 정리해보겠습니다.

### 시간 차이를 어떻게 계산할 것인가

- 시간을 단순히 o2.hour - o1.hour 한 값을 분으로 치환하기 위해 60을 곱하고 o2.min - o1.min을 한 값을 더한다면 오류가 생길 수 있습니다.
- 왜냐하면 endTime의 min값이 startTime의 min값보다 작을 수가 있기 때문이죠!
- 12:55 - 13:01 을 한다고 하면 1 - 55를 하게 되니 계산식이 이상해지겠죠??

```java
public int calcMusicRunTime(String start, String end) {
        String[] startTimeInfo = start.split(":");
        String[] endTimeInfo = end.split(":");

        int startTime = (Integer.parseInt(startTimeInfo[0]) * 60) + (Integer.parseInt(startTimeInfo[1]));
        int endTime = (Integer.parseInt(endTimeInfo[0]) * 60) + (Integer.parseInt(endTimeInfo[1]));
        
        return endTime - startTime;
    }
```

- 따라서 startTime과 endTime을 각각 분단위 계산을 끝마친 후에 빼주어야 합니다.

### 연속적으로 재생되는 것을 어떻게 구현할 것인가? + # 처리를 어떻게 할 것인가

- ABC 라는 음악이 10초간 재생이 된다면 ABCABCABCA 이 되겠죠?
- 저는 음계를 나누고 재생 시간만큼 순환하면서 StringBuilder를 이용해 붙여주면 될 것이라고 생각했습니다.
- 그런데 문제는 ABC#D 이런 식의 음악일 때인데요.
- A B C# D 로 각각 나누어주는 작업이 필요했고 반복적으로 순환하기 위해 음계의 개수(리스트의 사이즈)도 알아야 했습니다.

```java
ArrayList<String> sheet = new ArrayList<>();
partition(sheet, musicInfo[3]);

public void partition(ArrayList<String> list, String str) {
        for (int i = 0; i < str.length(); i++) {
            char current = str.charAt(i);
            if (current == '#') list.set(list.size()-1, list.get(list.size()-1) + String.valueOf('#'));
            else list.add(String.valueOf(current));
        };
    }
```

- 우선 문자열을 하나씩 순회하면서 **#** 이 나오면 바로 앞 문자에 붙여주는 식으로 각각의 음계들을 list에 담아주었습니다.

```java
StringBuilder sb = new StringBuilder();
int index = 0;
int count = musicRunTime;
while (count-- > 0) {
	index = index % sheet.size();
	sb.append(sheet.get(index));
	index++;
}
```

- 그리고 재생시간(musicRunTime)만큼 StringBuilder에 붙여주는 것이죠.
- index = index % sheet.size(); 이 부분은 \[A, B, C#, D\] 라는 음계 리스트가 있을 때 0번 인덱스인 A부터 3번 인덱스인 D까지를 계속 순회하기 위한 수식입니다.
	- 0 % 4 = 0
	- 1 % 4 = 1
	- 2 % 4 = 2
	- 3 % 4 = 3
	- 4 % 4 = 0
	- 5 % 4 = 1
	- 6 % 4 = 2
- 이런 식으로 순환이 되는 구조입니다.

### 음악이 입력된 멜로디를 포함하는지 어떻게 확인할 것인가(\#\ 처리를 포함해서)

```java
 String playedMelody = sb.toString();
while (playedMelody.contains(m)) {
	int idx = playedMelody.indexOf(m);
	if (idx + m.length() == playedMelody.length() || idx + m.length() < playedMelody.length() && playedMelody.charAt(idx + m.length()) != '#') {
		pq.offer(new Music(order++, musicRunTime, musicInfo[2]));
		break;
	}
	playedMelody = playedMelody.substring(idx + m.length());
}
```

- 이 부분은 좀 하드코딩한 느낌이 강합니다.
- 조건문도 복잡하고 가독성이 매우 떨어져서 좋은 방법은 아니라고 생각하는데... 딱히 더 나은 방법이 떠오르지 않았습니다 ㅠㅠ
- 우선 위에서 재생시간만큼 음계를 이어붙여준 문자열을 **playedMelody** 변수에 넣엏습니다.
- 그리고 **기억한 멜로디(m)** 이 해당 문자열에 포함되는지를 확인합니다.
- 이 때 주의할 것은 **기억한 멜로디가 ABC** 인데 **playedMelody가 ABC#** 이라면 이것 또한 조건문에서 true 루트를 타게 됩니다.
- 따라서 playedMelody의 뒷부분을 확인해서 **#** 이 아닐 때에 큐에 담아주는 형식입니다.
- 또한 indexOf는 가장 처음에 만나는 인덱스만 반환하는데 **playedMelody가 ABC#ABC#ABC** 인 경우에는 세 번째 ABC까지 확인을 해야 하므로 탐색을 한 뒤 큐에 들어갈 조건이 되지 않으면 탐색한 곳까지의 문자열을 삭제하는 작업(substring)을 하고 다시 탐색(while) 하도록 구현하였습니다.


### 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환하고 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환해야 하는 조건을 어떻게 구현할 것인가

```java
 public class Music implements Comparable<Music> {
        int index;
        int playingTime;
        String title;
        
        public Music(int index, int playingTime, String title) {
            this.index = index;
            this.playingTime = playingTime;
            this.title = title;
        }
        
        @Override
        public int compareTo(Music o) {
            if (o.playingTime == this.playingTime) {
                return this.index - o.index;
            } else {
                return o.playingTime - this.playingTime;
            }
        } 
    }
```

- 이 조건은 우선순위 큐와 Comparable을 이용해서 구현하였습니다.
- compareTo 메소드를 오버라이딩 하면서 재생시간이 더 길거나, 재생시간이 같다면 먼저 입력된 것(앞 index)을 우선순위로 하여 마지막에 pq.poll()을 해주면 되는 것이죠!
- compareTo 같은 경우 반환값이 양수일 때 자리를 바꾸고 0 이나 음수면 자리를 바꾸지 않는다는 점을 이용했습니다.


### 🔍 정답

```java
import java.util.*;

class Solution {    
    public String solution(String m, String[] musicinfos) {
        String musicTitle = "(None)";
        PriorityQueue<Music> pq = new PriorityQueue<>();

        int order = 0;
        for (String music : musicinfos) {
            String[] musicInfo = music.split(",");
            
            String startTimes = musicInfo[0];
            String endTimes = musicInfo[1];
            int musicRunTime = calcMusicRunTime(startTimes, endTimes);
        
            ArrayList<String> sheet = new ArrayList<>();
            partition(sheet, musicInfo[3]);
            
            StringBuilder sb = new StringBuilder();
            int index = 0;
            int count = musicRunTime;
            while (count-- > 0) {
                index = index % sheet.size();
                sb.append(sheet.get(index));
                index++;
            }
            
            String playedMelody = sb.toString();
            while (playedMelody.contains(m)) {
                int idx = playedMelody.indexOf(m);
                if (idx + m.length() == playedMelody.length() || idx + m.length() < playedMelody.length() && playedMelody.charAt(idx + m.length()) != '#') {
                    pq.offer(new Music(order++, musicRunTime, musicInfo[2]));
                    break;
                }
                playedMelody = playedMelody.substring(idx + m.length());
            }
        }
        
        if (!pq.isEmpty()) {
            Music music = pq.poll();
            musicTitle = music.title;
        }
        
        return musicTitle;
    }
    
    public void partition(ArrayList<String> list, String str) {
        for (int i = 0; i < str.length(); i++) {
            char current = str.charAt(i);
            if (current == '#') list.set(list.size()-1, list.get(list.size()-1) + String.valueOf('#'));
            else list.add(String.valueOf(current));
        };
    }
    
    public int calcMusicRunTime(String start, String end) {
        String[] startTimeInfo = start.split(":");
        String[] endTimeInfo = end.split(":");

        int startTime = (Integer.parseInt(startTimeInfo[0]) * 60) + (Integer.parseInt(startTimeInfo[1]));
        int endTime = (Integer.parseInt(endTimeInfo[0]) * 60) + (Integer.parseInt(endTimeInfo[1]));
        
        return endTime - startTime;
    }
    
    public class Music implements Comparable<Music> {
        int index;
        int playingTime;
        String title;
        
        public Music(int index, int playingTime, String title) {
            this.index = index;
            this.playingTime = playingTime;
            this.title = title;
        }
        
        @Override
        public int compareTo(Music o) {
            if (o.playingTime == this.playingTime) {
                return this.index - o.index;
            } else {
                return o.playingTime - this.playingTime;
            }
        } 
    }
}
```