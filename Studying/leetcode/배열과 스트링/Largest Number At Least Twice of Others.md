You are given an integer array `nums` where the largest integer is **unique**.

Determine whether the largest element in the array is **at least twice** as much as every other number in the array. If it is, return _the **index** of the largest element, or return_ `-1` _otherwise_.

**Example 1:**

**Input:** nums = [3,6,1,0]
**Output:** 1
**Explanation:** 6 is the largest integer.
For every other number in the array x, 6 is at least twice as big as x.
The index of value 6 is 1, so we return 1.

**Example 2:**

**Input:** nums = [1,2,3,4]
**Output:** -1
**Explanation:** 4 is less than twice the value of 3, so we return -1.

**Constraints:**

-   `2 <= nums.length <= 50`
-   `0 <= nums[i] <= 100`
-   The largest element in `nums` is unique.

---

```java
class Solution {
    public int dominantIndex(int[] nums) {
        int[] clone = nums.clone();
        Arrays.sort(clone);
        
        int largest = clone[clone.length-1];
        for (int i = 0; i < clone.length-1; i++) {
            if (clone[i] * 2 > largest) return -1;
        }
        
        return Arrays.stream(nums).boxed().collect(Collectors.toList()).indexOf(largest);
    }
}
```

```java
class Solution {
    public int dominantIndex(int[] nums) {
        int max = -1, index = -1, second = -1;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > max) {
                second = max;
                max = nums[i];
                index = i;
            } else if (nums[i] > second)
                second = nums[i];
        }
        return second * 2 <= max ? index : -1;
    }
}
```