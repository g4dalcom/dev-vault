# [문제링크](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## 📝 문제

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

**Input:** nums = [5,7,7,8,8,10], target = 8
**Output:** [3,4]

**Example 2:**

**Input:** nums = [5,7,7,8,8,10], target = 6
**Output:** [-1,-1]

**Example 3:**

**Input:** nums = [], target = 0
**Output:** [-1,-1]

**Constraints:**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `nums` is a non-decreasing array.
- `-109 <= target <= 109`

---

### 💡 풀이

- 오름차순으로 정렬된 배열에서 target 값이 차지하고 있는 인덱스의 범위를 알아내야 하는 문제로, 이분 탐색 중 upper bound 와 lower bound의 특성을 이용하여 풀어야 했습니다.
- **lower bound**는 찾고자 하는 **target 값 이상**이 처음 나오는 인덱스를 반환하게 되고
- **upper bound**는 **target 값을 초과하는** 첫 인덱스를 반환하게 됩니다.
- 따라서 upper bound - lower bound를 하게 되면 중복된 target값의 범위를 알 수 있게 되죠!
- \[5, 7, 7, 8, 8, 10\] 에서 target = 8 이라면 lower bound = 3, upper bound = 5 가 되는 식입니다.
- lower bound의 경우는 중복 원소 처리를 left 쪽에서 하게되고 upper bound는 right 쪽에서 하게 됩니다.
- 그리고 만약 target 이 배열의 모든 수들보다 크다면 nums.length 를 가리키게 되므로 처음 right는 nums.length를 초기값으로 가져야 합니다.
- 또한 찾는 값이 없을 경우 left와 right가 교차하면서 left를 반환하므로 left 와 right의 값이 같다면 찾는 값이 없다고 보고 -1을 넣어주어야 합니다!

### 🔍 정답

```java
class Solution {
    static int[] nums;
    static int target;
    static final String LOWER = "lower";
    static final String UPPER = "upper";

    public int[] searchRange(int[] nums, int target) {
        this.nums = nums;
        this.target = target;

        int left = 0;
        int right = nums.length;

        int lo = binarySearch(left, right, LOWER);
        int hi = binarySearch(left, right, UPPER);

        return lo == hi ? new int[]{-1, -1} : new int[]{lo, hi-1};
    }

    public int binarySearch(int left, int right, String bound) {
        while (left < right) {
            int mid = (left + right) / 2;

            if (bound == LOWER && nums[mid] >= target || bound == UPPER && nums[mid] > target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```