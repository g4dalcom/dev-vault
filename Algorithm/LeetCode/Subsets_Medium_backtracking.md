# [문제링크](https://leetcode.com/problems/subsets/)

## 📝 문제

Given an integer array `nums` of **unique** elements, return _all possible_ 

_subsets_

 _(the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

**Example 2:**

**Input:** nums = [0]
**Output:** [[],[0]]

**Constraints:**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- All the numbers of `nums` are **unique**.


---

### 💡 풀이

- 주어진 int형 배열들의 요소를 가지고 가능한 하위 집합을 모두 나타내면 되는 문제입니다.
- 주의할 점은 빈 배열도 하나 포함되어야 한다는 점이겠네요!
- \[1, 2\] 와 \[2, 1\] 같은 요소의 중복을 없애기 위해 조합의 방식을 이용하였습니다. 그렇기 때문에 탐색의 시작 인덱스를 제어할 수 있는 start 라는 변수를 사용했습니다.

```java
[[]]
[[], [1]]
[[], [1], [1, 2]]
[[], [1], [1, 2], [1, 2, 3]]
[[], [1], [1, 2], [1, 2, 3], [1, 3]]
[[], [1], [1, 2], [1, 2, 3], [1, 3], [2]]
[[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3]]
[[], [1], [1, 2], [1, 2, 3], [1, 3], [2], [2, 3], [3]]
```

- \[1, 2, 3\] 배열을 예로 들면 위 순서로 result에 들어가게 됩니다. 조합을 구하는 백트래킹과 같은 모습입니다!


### 🔍 정답

```java
class Solution {
    List<List<Integer>> result;
    int[] nums;

    public List<List<Integer>> subsets(int[] nums) {
        this.nums = nums;
        result = new ArrayList<>();

        combination(0, new ArrayList<>());
        return result;
    }

    public void combination(int start, ArrayList<Integer> list) {
        result.add(new ArrayList<>(list));
        
        for (int i = start; i < nums.length; i++) {
            list.add(nums[i]);
            combination(i+1, list);
            list.remove(list.size()-1);
        }
    }
}
```