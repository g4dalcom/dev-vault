# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/17686)

## 📝 문제

세 차례의 코딩 테스트와 두 차례의 면접이라는 기나긴 블라인드 공채를 무사히 통과해 카카오에 입사한 무지는 파일 저장소 서버 관리를 맡게 되었다.

저장소 서버에는 프로그램의 과거 버전을 모두 담고 있어, 이름 순으로 정렬된 파일 목록은 보기가 불편했다. 파일을 이름 순으로 정렬하면 나중에 만들어진 ver-10.zip이 ver-9.zip보다 먼저 표시되기 때문이다.

버전 번호 외에도 숫자가 포함된 파일 목록은 여러 면에서 관리하기 불편했다. 예컨대 파일 목록이 ["img12.png", "img10.png", "img2.png", "img1.png"]일 경우, 일반적인 정렬은 ["img1.png", "img10.png", "img12.png", "img2.png"] 순이 되지만, 숫자 순으로 정렬된 ["img1.png", "img2.png", "img10.png", img12.png"] 순이 훨씬 자연스럽다.

무지는 단순한 문자 코드 순이 아닌, 파일명에 포함된 숫자를 반영한 정렬 기능을 저장소 관리 프로그램에 구현하기로 했다.

소스 파일 저장소에 저장된 파일명은 100 글자 이내로, 영문 대소문자, 숫자, 공백(" "), 마침표("."), 빼기 부호("-")만으로 이루어져 있다. 파일명은 영문자로 시작하며, 숫자를 하나 이상 포함하고 있다.

파일명은 크게 HEAD, NUMBER, TAIL의 세 부분으로 구성된다.

- HEAD는 숫자가 아닌 문자로 이루어져 있으며, 최소한 한 글자 이상이다.
- NUMBER는 한 글자에서 최대 다섯 글자 사이의 연속된 숫자로 이루어져 있으며, 앞쪽에 0이 올 수 있다. `0`부터 `99999` 사이의 숫자로, `00000`이나 `0101` 등도 가능하다.
- TAIL은 그 나머지 부분으로, 여기에는 숫자가 다시 나타날 수도 있으며, 아무 글자도 없을 수 있다.

|파일명|HEAD|NUMBER|TAIL|
|---|---|---|---|
|`foo9.txt`|`foo`|`9`|`.txt`|
|`foo010bar020.zip`|`foo`|`010`|`bar020.zip`|
|`F-15`|`F-`|`15`|(빈 문자열)|

파일명을 세 부분으로 나눈 후, 다음 기준에 따라 파일명을 정렬한다.

- 파일명은 우선 HEAD 부분을 기준으로 사전 순으로 정렬한다. 이때, 문자열 비교 시 대소문자 구분을 하지 않는다. `MUZI`와 `muzi`, `MuZi`는 정렬 시에 같은 순서로 취급된다.
- 파일명의 HEAD 부분이 대소문자 차이 외에는 같을 경우, NUMBER의 숫자 순으로 정렬한다. 9 < 10 < 0011 < 012 < 13 < 014 순으로 정렬된다. 숫자 앞의 0은 무시되며, 012와 12는 정렬 시에 같은 같은 값으로 처리된다.
- 두 파일의 HEAD 부분과, NUMBER의 숫자도 같을 경우, 원래 입력에 주어진 순서를 유지한다. `MUZI01.zip`과 `muzi1.png`가 입력으로 들어오면, 정렬 후에도 입력 시 주어진 두 파일의 순서가 바뀌어서는 안 된다.

무지를 도와 파일명 정렬 프로그램을 구현하라.

### 입력 형식

입력으로 배열 `files`가 주어진다.

- `files`는 1000 개 이하의 파일명을 포함하는 문자열 배열이다.
- 각 파일명은 100 글자 이하 길이로, 영문 대소문자, 숫자, 공백(" "), 마침표("."), 빼기 부호("-")만으로 이루어져 있다. 파일명은 영문자로 시작하며, 숫자를 하나 이상 포함하고 있다.
- 중복된 파일명은 없으나, 대소문자나 숫자 앞부분의 0 차이가 있는 경우는 함께 주어질 수 있다. (`muzi1.txt`, `MUZI1.txt`, `muzi001.txt`, `muzi1.TXT`는 함께 입력으로 주어질 수 있다.)

### 출력 형식

위 기준에 따라 정렬된 배열을 출력한다.

### 입출력 예제

입력: ["img12.png", "img10.png", "img02.png", "img1.png", "IMG01.GIF", "img2.JPG"]  
출력: ["img1.png", "IMG01.GIF", "img02.png", "img2.JPG", "img10.png", "img12.png"]

입력: ["F-5 Freedom Fighter", "B-50 Superfortress", "A-10 Thunderbolt II", "F-14 Tomcat"]  
출력: ["A-10 Thunderbolt II", "B-50 Superfortress", "F-5 Freedom Fighter", "F-14 Tomcat"]

---

### 💡 풀이

- 카카오 알고리즘은 특히나 문제를 꼼꼼하게 읽어야 하는 것 같습니다..
- 구현을 다 해놓고 에러가 떠서 살펴보면 조건을 제대로 이해하지 못했거나 누락한 경우가 종종 있었고 처음에 생각지 못한 고민거리가 생겨서 고쳐야 하는 경우도 많았습니다.
- 이 문제도 여러가지로 시행착오가 많았어요 ㅠㅠ
- 우선 Head와 Number 부분을 분리해야 합니다. 두 부분만 확실히 분리가 된다면 Tail은 신경쓰지 않아도 됩니다.
- Head는 **숫자가 아닌 대소문자** 와 **공백, -, .** 으로 이루어져 있습니다. 대소문자는 구별하지 않으므로 A와 a를 같다고 보아야 하고 공백과 특수문자가 포함되기 때문에 단순히 **A.compareTo(B)** 를 한다거나 **A.toLowerCase()** 를 할 수 없습니다.
- Number는 숫자로 이루어져있는데 **최대 다섯글자의 연속된 숫자**입니다. 처음에 저는 문자열을 역방향 순회하면서 숫자가 나오면 lastHead와 나중에 구한 인덱스를 Number로 두려고 했으나 최대 다섯글자라는 조건과 Tail에서도 숫자가 나올 수 있다는 조건이 있기 때문에 다른 방법을 사용하였습니다.

```java
int lastHead = getLastHeadIndex(file);
String head = replaceSpace(file.substring(0, lastHead));

public int getLastHeadIndex(String file) {
        int answer = 0;
        for (int i = 0; i < file.length(); i++) {
            int current = file.charAt(i);
            if (current > 47 && current < 58) {
                answer = i;
                break;
            }
        }
        return answer;
    }
    
public String replaceSpace(String str) {
        return str.replaceAll(" ", "&");
    }
```

- 먼저 Head는 문자열을 정방향 순회하다가 숫자가 나오면 0부터 해당 인덱스까지 substring하면 쉽게 구할 수가 있습니다.
- 그리고 이후에 Comparable 메소드로 문자 하나씩 비교를 할 것인데 이 때 공백은 charAt으로 셀렉트가 되지 않아서 & 문자로 바꾸는 작업도 추가하였습니다.

```java
int number = Integer.parseInt(file.substring(lastHead, lastNumber));

public int getLastNumberIndex(String file, int start) {
        int answer = start;
        int end = Math.min(start+5, file.length());
        for (int i = start; i < end; i++) {
            int current = file.charAt(i);
            if (current > 47 && current < 58) answer = i;
            else break;
        }

        return answer + 1;
    }
```

- Number는 최대 연속된 다섯개의 숫자이므로 첫 숫자인 **start** 부터 이후 5개까지만 탐색하면 됩니다. 그런데 start + 5보다 file의 길이가 더 작을 수도 있으므로 Math.min 메소드를 이용하였습니다.
- answer + 1을 한 이유는 substring은 두 번째 인자 이전까지 자르기 때문입니다.

```java
public class File implements Comparable<File> {
        int index;
        String fileName;
        String head;
        int number;
        
        public File (int index, String fileName, String head, int number) {
            this.index = index;
            this.fileName = fileName;
            this.head = head;
            this.number = number;
        }
        
        @Override
        public int compareTo(File o) {
            int base = 0;
            int compare = 0;
            int range = Math.min(head.length(), o.head.length());       
            
            for (int i = 0; i < range; i++) {
                base = head.charAt(i) > 47 ? Character.toLowerCase(head.charAt(i)) : head.charAt(i);
                compare = o.head.charAt(i) > 47 ? Character.toLowerCase(o.head.charAt(i)) : o.head.charAt(i);
                if (base != compare) return base - compare;
            }
            if (head.length() != o.head.length()) return head.length() - o.head.length();

            if (number != o.number) return number - o.number;

            return index - o.index;
        }
    }
```

- 마지막으로 순서 비교는 클래스에 Comparable을 구현했습니다.
- 굉장히 지저분하고 복잡한데 ㅠㅠ 우선 **문자열을 문자 하나씩 비교하는 작업**과 **문자의 길이가 다를 경우 문자의 길이가 긴 것을 뒤로 보내는 작업** 그리고 **Number의 단순 비교**와 마지막으로 **입력된 순서(index)를 비교**해서 우선순위 큐에 담아주었습니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public String[] solution(String[] files) {
        int index = 0;
        String[] answer = new String[files.length];
        PriorityQueue<File> pq = new PriorityQueue<>();
        
        for (String file : files) {
            int lastHead = getLastHeadIndex(file);
            int lastNumber = getLastNumberIndex(file, lastHead);
            
            String head = replaceSpace(file.substring(0, lastHead));
            int number = Integer.parseInt(file.substring(lastHead, lastNumber));

            pq.offer(new File(index++, file, head, number));  
        }
        
        int idx = 0;
        while (!pq.isEmpty()) {
            File f = pq.poll();
            answer[idx++] = f.fileName;
        }
        
        return answer;
    }
    
    public int getLastHeadIndex(String file) {
        int answer = 0;
        for (int i = 0; i < file.length(); i++) {
            int current = file.charAt(i);
            if (current > 47 && current < 58) {
                answer = i;
                break;
            }
        }
        return answer;
    }
    
    public int getLastNumberIndex(String file, int start) {
        int answer = start;
        int end = Math.min(start+5, file.length());
        for (int i = start; i < end; i++) {
            int current = file.charAt(i);
            if (current > 47 && current < 58) answer = i;
            else break;
        }

        return answer + 1;
    }
    
    public String replaceSpace(String str) {
        return str.replaceAll(" ", "&");
    }
    
    public class File implements Comparable<File> {
        int index;
        String fileName;
        String head;
        int number;
        
        public File (int index, String fileName, String head, int number) {
            this.index = index;
            this.fileName = fileName;
            this.head = head;
            this.number = number;
        }
        
        @Override
        public int compareTo(File o) {
            int base = 0;
            int compare = 0;
            int range = Math.min(head.length(), o.head.length());       
            
            for (int i = 0; i < range; i++) {
                base = head.charAt(i) > 47 ? Character.toLowerCase(head.charAt(i)) : head.charAt(i);
                compare = o.head.charAt(i) > 47 ? Character.toLowerCase(o.head.charAt(i)) : o.head.charAt(i);
                if (base != compare) return base - compare;
            }
            if (head.length() != o.head.length()) return head.length() - o.head.length();

            if (number != o.number) return number - o.number;

            return index - o.index;
        }
    }
}
```