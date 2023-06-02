Given an integer array `nums` and an integer `k`, return _the_ `kth` _largest element in the array_.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

You must solve it in `O(n)` time complexity.

**Example 1:**

**Input:** nums = [3,2,1,5,6,4], k = 2
**Output:** 5

**Example 2:**

**Input:** nums = [3,2,3,1,2,4,5,5,6], k = 4
**Output:** 4

**Constraints:**

- `1 <= k <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

---

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int result = 0;
        
        if (nums.length < 2) {
            result = nums[0];
            return result;
        }
        
        for (int i = nums.length/2 -1; i >= 0; i--) {
            heapify(nums, i, nums.length);
        }
        
        
        for (int i = nums.length-1; i >= 0; i--) {
            if (--k == 0) {
                result = nums[0];
                break;
            }
            swap(nums, 0, i);
            heapify(nums, 0, i);
        }
        
        return result;
    }
    
    public void heapify(int[] nums, int parentIdx, int size) {
        int left;
        int right;
        int largest;
        
        while ((parentIdx * 2) + 1 < size) {
            left = parentIdx * 2 + 1;
            right = parentIdx * 2 + 2;
            largest = parentIdx;
            
            if (nums[left] > nums[largest]) largest = left;
            if (right < size && nums[right] > nums[largest]) largest = right;
            
            if (largest != parentIdx) {
                swap(nums, parentIdx, largest);
                parentIdx = largest;
            }
            else return;
        }
    }
    
    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```