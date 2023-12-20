# [문제링크](https://leetcode.com/problems/maximum-subarray/)

## 📝 문제

Given an integer array `nums`, find the 

subarray

 with the largest sum, and return _its sum_.

**Example 1:**

**Input:** nums = [-2,1,-3,4,-1,2,1,-5,4]
**Output:** 6
**Explanation:** The subarray [4,-1,2,1] has the largest sum 6.

**Example 2:**

**Input:** nums = [1]
**Output:** 1
**Explanation:** The subarray [1] has the largest sum 1.

**Example 3:**

**Input:** nums = [5,4,-1,7,8]
**Output:** 23
**Explanation:** The subarray [5,4,-1,7,8] has the largest sum 23.

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

---

### 💡 풀이

- **현재까지의 sum + 현재 value** 와 **현재 value** 두 가지를 비교했을 때 현재 value가 더 크다면 이전까지의 sum은 할 필요가 없을 것입니다.
- 따라서 위 두 수를 비교해서 dp 배열에 갱신해주는 식으로 구현하였습니다.

### 🔍 정답

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int[] dp = new int[nums.length];
        dp[0] = nums[0];
        int sum = 0;
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] < sum + nums[i]) {
                dp[i] = sum + nums[i];
                sum += nums[i];
            } else {
                dp[i] = nums[i];
                sum = nums[i];
            }
            max = Math.max(max, dp[i]);
        }
        return max;
    }
}
```


### 💡 다른 정답

```java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSum = nums[0];
        int currentSum = nums[0];

        for (int i = 1; i < nums.length; i++) {
            currentSum = Math.max(nums[i], currentSum + nums[i]);
            maxSum = Math.max(maxSum, currentSum);
        }

        return maxSum;
   
```

- 사실 이전 dp값을 이용해서 연산을 해야 하는 것이 아니라서 굳이 dp배열이 필요하지 않았습니다. 
- **현재까지의 sum + 현재 value** 와 **현재 value** 중 큰 값을 하나의 변수에 갱신해가는 게 좀 더 깔끔한 풀이인 것 같네요!