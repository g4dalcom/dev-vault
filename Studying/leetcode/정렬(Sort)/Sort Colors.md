Given an array `nums` with `n` objects colored red, white, or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

**Example 1:**

**Input:** nums = [2,0,2,1,1,0]
**Output:** [0,0,1,1,2,2]

**Example 2:**

**Input:** nums = [2,0,1]
**Output:** [0,1,2]

**Constraints:**

- `n == nums.length`
- `1 <= n <= 300`
- `nums[i]` is either `0`, `1`, or `2`.

**Follow up:** Could you come up with a one-pass algorithm using only constant extra space?

---

### 선택 정렬

```java
class Solution {
    public void sortColors(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            int base = i;
            
            for (int j = i+1; j < nums.length; j++) {
                if (nums[base] > nums[j]) base = j;    
            }
            
            int temp = nums[i];
            nums[i] = nums[base];
            nums[base] = temp;
        }
    }
}
```


### 카운팅 정렬

```java
class Solution {
    public void sortColors(int[] nums) {
        int max = Arrays.stream(nums).max().getAsInt();
        int[] counting = new int[max+1];
        int[] result = new int[nums.length];
        
        for (int num : nums) {
            counting[num]++;
        }
        
        for (int i = 1; i < counting.length; i++) {
            counting[i] += counting[i-1];
        }
        
        for (int i = nums.length-1; i >= 0; i--) {
            int value = nums[i];
            counting[value]--;
            result[counting[value]] = value;
        }
        
        for (int i = 0; i < nums.length; i++) {
            nums[i] = result[i];
        }
    }
}
```