# [문제링크](https://leetcode.com/problems/combination-sum/)

## 📝 문제

Given an array of **distinct** integers `candidates` and a target integer `target`, return _a list of all **unique combinations** of_ `candidates` _where the chosen numbers sum to_ `target`_._ You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the 

frequency

 of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1:**

**Input:** candidates = [2,3,6,7], target = 7
**Output:** [[2,2,3],[7]]
**Explanation:**
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

**Example 2:**

**Input:** candidates = [2,3,5], target = 8
**Output:** [[2,2,2,2],[2,3,3],[3,5]]

**Example 3:**

**Input:** candidates = [2], target = 1
**Output:** []

**Constraints:**

- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- All elements of `candidates` are **distinct**.
- `1 <= target <= 40`

---

### 💡 풀이

- 중복 조합을 구현하는 문제입니다. 
- 중복 조합은 조합과 다르게 visit 배열을 사용하지 않고 backtracking 로직에 start 인덱스를 넘겨서 start보다 이전 인덱스는 탐색하지 않도록 합니다.
- \[2, 3, 5\] 로 target = 8 을 만드는 방법은, \[2, 2, 2, 2\] 와 \[2, 2, 3\], \[3, 5\] 가 있는데 이 때 2 3 2나 5 3 과 같은 탐색을 피하기 위해 start 인덱스를 넘기게 됩니다.

### 🔍 정답

```java
class Solution {
    static List<List<Integer>> result;
    static List<Integer> combinations;
    static int sum = 0;

    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        result = new ArrayList<>();
        combinations = new ArrayList<>();
        backtracking(0, candidates, target);

        return result;
    }

    public void backtracking(int start, int[] candidates, int target) {
        if (sum > target) return;

        if (sum == target && !result.contains(combinations)) {
            result.add(new ArrayList<>(combinations));
            return;
        }

        for (int i = start; i < candidates.length; i++) {
            combinations.add(candidates[i]);
            sum += candidates[i];
            backtracking(i, candidates, target);
            combinations.remove(Integer.valueOf(candidates[i]));
            sum -= candidates[i];
        }
    }
}
```