# [문제링크](https://leetcode.com/problems/merge-intervals/)

## 📝 문제

Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.

**Example 1:**

**Input:** intervals = [[1,3],[2,6],[8,10],[15,18]]
**Output:** [[1,6],[8,10],[15,18]]
**Explanation:** Since intervals [1,3] and [2,6] overlap, merge them into [1,6].

**Example 2:**

**Input:** intervals = [[1,4],[4,5]]
**Output:** [[1,5]]
**Explanation:** Intervals [1,4] and [4,5] are considered overlapping.

**Constraints:**

- `1 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= starti <= endi <= 104`

---

### 💡 풀이

- \[start, end\] 라는 일정한 간격을 두고 있는 배열들이 주어지는데 이 간격들이 겹치지 않도록 재배열해서 리턴해야 하는 문제입니다.
- \[\[1, 3\], \[2, 6\], \[8, 10\]\] 일 때, 1~3 사이에 2~6 중 2, 3이 포함되므로 둘을 합쳐서 \[1, 6\] 으로 만들어주고 그 다음 8, 10은 겹치지 않으므로 그대로 사용할 수 있습니다.
- 그러면 어떤 조건에서 두 배열을 합쳐줘야 하는지 먼저 파악을 해야겠네요!
- 배열의 1번 인덱스인 end 값이 그 다음 start보다 크거나 같다면 두 배열은 겹치는 부분이 존재하는 것입니다. 그러므로 이 조건으로 분기를 해주면 될 것 같았습니다.
- intervals 배열이 오름차순으로 주어진다는 조건이 없기 때문에 가장 먼저 오름차순 정렬을 해줍니다.

```java
Arrays.sort(intervals, (o1, o2) -> o1[0] == o2[0] ? o1[1] - o2[1] : o1[0] - o2[0]);
```

- 그리고 비교해야 하는 배열은 탐색이 끝난 마지막 배열과 그 다음 배열이므로 stack을 이용해서 비교하고 마지막에 리턴할 배열에 넣어줄 때에는 역순으로 넣어서 반환하였습니다.
- 만약 두 배열의 겹치는 부분이 존재해서 배열을 합쳐야 하는 경우에는 두 배열의 end를 비교해서 더 큰 부분을 넣어주어야 합니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int[][] merge(int[][] intervals) {
        Arrays.sort(intervals, (o1, o2) -> o1[0] == o2[0] ? o1[1] - o2[1] : o1[0] - o2[0]);
        Stack<Interval> stack = new Stack<>();

        for (int i = 0; i < intervals.length; i++) {
            int start = intervals[i][0];
            int end = intervals[i][1];
            
            if (stack.isEmpty() || stack.peek().end < start) {
                stack.push(new Interval(start, end));
            } else {
                Interval in = stack.pop();
                in.end = Math.max(in.end, end);
                stack.push(in);
            }
        }

        int[][] result = new int[stack.size()][2];

        for (int i = result.length-1; i >= 0; i--) {
            Interval in = stack.pop();
            result[i] = new int[]{in.start, in.end};
        }

        return result;
    }

    public class Interval {
        int start, end;

        public Interval(int start, int end) {
            this.start = start;
            this.end = end;
        }
    }
}
```