## 📝 Remove Element(요소 제거)

- 정수 배열 nums와 정수 val이 주어지면 nums에서 val의 모든 항목을 제자리에서 제거합니다. 요소의 순서는 변경될 수 있습니다. 그런 다음 val과 같지 않은 nums의 요소 수를 반환합니다.
- val이 k와 같지 않은 nums의 요소 수를 고려하여 수락하려면 다음 작업을 수행해야 합니다.
	- nums의 첫 번째 k 요소가 val과 같지 않은 요소를 포함하도록 배열 nums를 변경합니다. nums의 나머지 요소는 nums의 크기만큼 중요하지 않습니다. 
	- k를 반환합니다.

### Custom judge :

```java
int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

### Example 1 :

```text
Input: nums = [3,2,2,3], val = 3
Output: 2, nums = [2,2,_,_]
Explanation: 함수는 nums의 처음 두 요소가 2인 k = 2를 반환해야 합니다. 반환된 k 뒤에 무엇을 남기는지는 중요하지 않습니다(따라서 밑줄입니다).
```

### Example 2 :

```text
Input: nums = [0,1,2,2,3,0,4,2], val = 2
Output: 5, nums = [0,1,4,0,3,_,_,_]
Explanation: 함수는 0, 0, 1, 3 및 4를 포함하는 nums의 처음 5개 요소와 함께 k = 5를 반환해야 합니다. 5개 요소는 임의의 순서로 반환될 수 있습니다. 반환된 k 뒤에 무엇을 남기는지는 중요하지 않습니다(따라서 밑줄입니다).
```

### Constraints :

- `0 <= nums.length <= 100`
- `0 <= nums[i] <= 50`
- `0 <= val <= 100`

---

### 🔍 정답

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int cnt = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == val) {
                nums[i] = 51;
                cnt++;
            }
        }
        
        Arrays.sort(nums);
        return nums.length - cnt;
    }
}
```

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int k = 0;
        
        for (int num: nums) {
            if (num != val) {
                nums[k] = num;
                k++;
            }
        }
        
        return k;
    }
}
```


### ⭐ 다른 정답

```java
class Solution {
    public int removeElement(int[] nums, int val) {
        int i = 0;
        for (int j = 0; j < nums.length; j++) {
            if (nums[j] != val) {
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
                i++;
            }
        }
        return i;
    }
}
```


## 📝 Remove Duplicates from Sorted Array(정렬된 배열에서 중복 제거)

- 감소하지 않는 순서로 정렬된 정수 배열 nums가 주어지면 각 고유 요소가 한 번만 나타나도록 중복 항목을 제자리에서 제거합니다.
- 요소의 상대적인 순서는 동일하게 유지되어야 합니다. 그런 다음 고유한 요소의 수를 숫자로 반환합니다.

### Custom judge :

```java
int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

### Example 1 :

```text
Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: 함수는 nums의 처음 두 요소가 각각 1과 2인 k = 2를 반환해야 합니다. 반환된 k 뒤에 무엇을 남기는지는 중요하지 않습니다(따라서 밑줄입니다).
```

### Example 2 :

```text
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: 함수는 nums의 처음 5개 요소가 각각 0, 1, 2, 3, 4인 k = 5를 반환해야 합니다. 반환된 k 뒤에 무엇을 남기는지는 중요하지 않습니다(따라서 밑줄입니다).
```

### Constraints :

- `1 <= nums.length <= 3 * 104`
- `-100 <= nums[i] <= 100`
- `nums` is sorted in **non-decreasing** order.

---

### 🔍 정답

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int cnt = 0;
        
        for (int n : nums) {
            if (cnt == 0 || nums[cnt-1] < n) {
                nums[cnt++] = n;
            }
        }
        
        return cnt;
    }
}
```