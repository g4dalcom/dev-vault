Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

**Example 1:**

**Input:** nums = [1,2,3,4,5,6,7], k = 3
**Output:** [5,6,7,1,2,3,4]
**Explanation:**
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

**Example 2:**

**Input:** nums = [-1,-100,3,99], k = 2
**Output:** [3,99,-1,-100]
**Explanation:** 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]

**Constraints:**

- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`
- `0 <= k <= 105`

**Follow up:**

- Try to come up with as many solutions as you can. There are at least **three** different ways to solve this problem.
- Could you do it in-place with `O(1)` extra space?

---

int[] nums = \[1, 2, 3, 4, 5, 6, 7\], k = 3 이라고 하면,
nums.length - k 인 5부터 nums.length - 1인 7까지의 수들을 앞으로 보내야 한다.
다시 말해서 \[5, 6, 7, 1, 2, 3, 4\] 가 되어야 하는 것이다.

1. 우선 \[1, 2, 3, 4\] 와  \[5, 6, 7\] 을 두 덩어리로 나누고
2. 각각을 뒤집는다. \[4, 3, 2, 1\], \[7, 6, 5\]
3. 그리고 두 배열을 하나로 이어서 뒤집으면 원하는 답을 얻을 수 있다.

이 로직을 바꾸어보면,

1. 배열을 뒤집는다. \[7, 6, 5, 4, 3, 2, 1\]
2. 두 덩어리로 나눈 배열을 각각 뒤집는다.
\[5, 6, 7\], \[1, 2, 3, 4\]

코드로 표현하면 아래와 같다!

```java
class Solution {
    public void rotate(int[] nums, int k) {
        int rotate = k % nums.length;
        
        rotating(nums, 0, nums.length-1);
        rotating(nums, 0, rotate-1);
        rotating(nums, rotate, nums.length-1);
        
    }
    
    public void rotating(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start] = nums[end];
            nums[end] = temp;
            start++;
            end--;
        }
    }
}
```