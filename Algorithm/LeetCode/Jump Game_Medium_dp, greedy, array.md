# [문제링크](https://leetcode.com/problems/jump-game/)

## 📝 문제

You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` _if you can reach the last index, or_ `false` _otherwise_.

**Example 1:**

**Input:** nums = [2,3,1,1,4]
**Output:** true
**Explanation:** Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**

**Input:** nums = [3,2,1,0,4]
**Output:** false
**Explanation:** You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

**Constraints:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 105`

---

### 💡 풀이

- 이전에 풀었던 Jump Game II 와 비슷한 문제이나 마지막 인덱스에 도달할 수 있는지 여부만을 리턴해주면 되는 문제입니다.
- 단순하게  i + nums[i] 가 최대 인덱스보다 크거나 같다면 도달 가능하리라 생각할 수 있지만 중간에 0이 있어서 더이상 앞으로 나아가지 못하는 경우가 있을 수 있습니다.
- \[1, 3, 2, 0, 0, 5, 1, 4\] 는 5번 인덱스에서 최대 인덱스에 도달 가능하지만 애초에 5번 인덱스까지 갈 수가 없기 때문에 false를 리턴해야 하는 것입니다.
- 따라서 최대 거리를 계속해서 갱신해주되 현재 인덱스가 최대 거리보다 커지면 도달할 수 없는 인덱스에 있는 것이므로 이 때에도 false를 리턴하도록 예외처리를 해주었습니다.
- 그리고 마지막 인덱스에 도달하는지 여부는 마지막 인덱스 -1 에서 이미 결정이 되기 때문에 for문 탐색의 범위를 i < nums.length-1 로 해야 하지만 nums의 길이가 1인 경우에도 for문을 한 번 통과시키기 위해서 i < nums.length; 까지 탐색을 하고 i 가 nums.length-1 인 경우 for문을 빠져나오도록 구현하였습니다.


### 🔍 정답

```java
class Solution {
    public boolean canJump(int[] nums) {
        int maxRange = 0;
        for (int i = 0; i < nums.length; i++) {
            if (maxRange >= nums.length-1) return true;
            else if (i == nums.length-1) break;
            else if (maxRange < i) break;
            else {
                maxRange = Math.max(maxRange, i + nums[i]);
            }
        }   
        return false;
    }
}
```