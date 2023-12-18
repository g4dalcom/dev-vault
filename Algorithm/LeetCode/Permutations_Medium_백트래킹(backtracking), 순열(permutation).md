# [문제링크](https://leetcode.com/problems/permutations/)

## 📝 문제

Given an array `nums` of distinct integers, return _all the possible permutations_. You can return the answer in **any order**.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

**Example 2:**

**Input:** nums = [0,1]
**Output:** [[0,1],[1,0]]

**Example 3:**

**Input:** nums = [1]
**Output:** [[1]]

**Constraints:**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- All the integers of `nums` are **unique**.

---

### 💡 풀이

- 순열을 구현하는 문제입니다.
- 조합과는 다르게 순열은 같은 요소들의 집합이어도 순서가 다르다면 다른 것으로 보기 때문에 백트래킹을 할 때 탐색 인덱스를 항상 0부터 놓아야 합니다.
- 그래야 \[1, 2, 3\] 일 때 2 1 3 과 같은 역순 탐색을 할 수 있으니까요!


### 🔍 정답

```java
class Solution {
    static List<List<Integer>> result;
    static List<Integer> list;
    static boolean[] visit;

    public List<List<Integer>> permute(int[] nums) {
        result = new ArrayList<>();
        list = new ArrayList<>();
        visit = new boolean[nums.length];
        permutation(0, nums);

        return result;
    }

    public void permutation(int depth, int[] nums) {
        if (depth == nums.length) {
            result.add(new ArrayList<>(list));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            if (!visit[i]) {
                visit[i] = true;
                list.add(nums[i]);
                permutation(depth+1, nums);
                visit[i] = false;
                list.remove(list.size()-1);
            }
        }
    }
}
```