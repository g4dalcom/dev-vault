# [문제링크](https://www.acmicpc.net/problem/11000)

## 📝 문제

수강신청의 마스터 김종혜 선생님에게 새로운 과제가 주어졌다. 

김종혜 선생님한테는 Si에 시작해서 Ti에 끝나는 N개의 수업이 주어지는데, 최소의 강의실을 사용해서 모든 수업을 가능하게 해야 한다. 

참고로, 수업이 끝난 직후에 다음 수업을 시작할 수 있다. (즉, Ti ≤ Sj 일 경우 i 수업과 j 수업은 같이 들을 수 있다.)

수강신청 대충한 게 찔리면, 선생님을 도와드리자!

## 입력

첫 번째 줄에 N이 주어진다. (1 ≤ N ≤ 200,000)

이후 N개의 줄에 Si, Ti가 주어진다. (0 ≤ Si < Ti ≤ 109)

## 출력

강의실의 개수를 출력하라.

## 예제 입력 1 

3
1 3
2 4
3 5

## 예제 출력 1

2

---

### 🔍 정답

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());    // 수업의 수

        Lecture[] lectures = new Lecture[N];
        for (int i = 0; i < N; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int start = Integer.parseInt(st.nextToken());
            int end = Integer.parseInt(st.nextToken());

            lectures[i] = new Lecture(start, end);
        }

        // 시작 시간이 빠른 순서로 정렬하되, 시작 시간이 같다면 종료 시간이 더 빠른 순서로 정렬!
        Arrays.sort(lectures, (a, b) -> a.start == b.start ? a.end - b.end : a.start - b.start);

        /**
         * 우선순위큐에 종료시간을 넣고 다른 수업의 시작시간과 비교!
         * 만약 다른 수업의 시작 시간이 같거나 크다면 같은 강의실에서 수업이 가능한 것이므로 시간을 바꿔주고(pq.poll() + pq.offer())
         * 같은 강의실에서 불가능한 수업이라면 새롭게 큐에 넣는다.(pq.offer())
         * 마지막에 큐의 크기를 반환하면 필요한 강의실 개수가 나옴!
         */
        PriorityQueue<Lecture> pq = new PriorityQueue<>();
        pq.offer(lectures[0]);

        for (int i = 1; i < N; i++) {
            if (pq.peek().end <= lectures[i].start) {
                pq.poll();
            }
            pq.offer(lectures[i]);
        }

        System.out.println(pq.size());
    }

    public static class Lecture implements Comparable<Lecture> {
        int start, end;

        public Lecture(final int start, final int end) {
            this.start = start;
            this.end = end;
        }

        // 우선순위 큐의 우선순위를 end값으로 주기 위한 오버라이딩
        @Override
        public int compareTo(final Lecture o) {
            return end - o.end;
        }
    }
}
```