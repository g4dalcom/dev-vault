## 📝 Replace Elements with Greatest Element on Right Side(요소를 오른쪽에서 가장 큰 요소로 바꾸기)

- 배열 arr이 주어지면 해당 배열의 모든 요소를 ​​오른쪽에 있는 요소 중 가장 큰 요소로 바꾸고 마지막 요소를 -1로 바꿉니다. 
- 그런 다음 배열을 반환합니다.

### Example 1 :

```text
Input: arr = [17,18,5,4,6,1]
Output: [18,6,6,6,1,-1]
Explanation: 
- index 0 --> the greatest element to the right of index 0 is index 1 (18).
- index 1 --> the greatest element to the right of index 1 is index 4 (6).
- index 2 --> the greatest element to the right of index 2 is index 4 (6).
- index 3 --> the greatest element to the right of index 3 is index 4 (6).
- index 4 --> the greatest element to the right of index 4 is index 5 (1).
- index 5 --> there are no elements to the right of index 5, so we put -1.
```

### Example 2 :

```text
Input: arr = [400]
Output: [-1]
Explanation: There are no elements to the right of index 0.
```

### Constraints :

- `1 <= arr.length <= 104`
- `1 <= arr[i] <= 105`

---

### 🔍 정답

```java
class Solution {
    public int[] replaceElements(int[] arr) {
        for (int i = 0; i < arr.length-1; i++) {
            int cur = 0;
            for (int j = i+1; j < arr.length; j++) {
                if (cur < arr[j]) {
                    arr[i] = arr[j];
                    cur = arr[j];
                }
            }
        }
        arr[arr.length-1] = -1;
        
        return arr;
    }
}
```


### ⭐ 다른 정답

```java
class Solution {
    public int[] replaceElements(int[] arr) {
        int max = -1;
        for (int i = arr.length-1; i >= 0; i--) {
            int tmp = arr[i];
            arr[i] = max;
            max = Math.max(arr[i], tmp);
        }
        
        return arr;
    }
}
```


## 📝 Remove Duplicates from Sorted Array(정렬된 배열에서 중복 제거)

- 감소하지 않는 순서로 정렬된 정수 배열 nums가 주어지면 각 고유 요소가 한 번만 나타나도록 중복 항목을 제자리에서 제거합니다.
- 요소의 상대적인 순서는 동일하게 유지되어야 합니다. 
- 그런 다음 고유한 요소의 수를 숫자로 반환합니다.

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


### ⭐ 다른 정답

```java
public int removeDuplicates(int[] nums) {

  if (nums == null) {
      return 0;
  }

  int writePointer = 1;

  for (int readPointer = 1; readPointer < nums.length; readPointer++) {
      if (nums[readPointer] != nums[readPointer - 1]) {
          nums[writePointer] = nums[readPointer];
          writePointer++;
      }
  }
  return writePointer;
}
```


## 📝  Move Zeroes(0 이동)

- 정수 배열 nums가 주어지면 0이 아닌 요소의 상대적인 순서를 유지하면서 모든 0을 끝으로 이동합니다. 
- 배열의 복사본을 만들지 않고 이 작업을 제자리에서 수행해야 합니다.

### Example 1 :

```text
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]
```

### Example 2 :

```text
Input: nums = [0]
Output: [0]
```

### Constraints :

- `1 <= nums.length <= 104`
- `-231 <= nums[i] <= 231 - 1`

---

### 🔍 정답

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int lo = 0;
        int hi = 0;
        
        if (nums.length == 1) return;
        
        while (true) {
            
           while (lo < nums.length && nums[lo] != 0) {
                lo++;
            }
            
            hi = lo;

            while (hi < nums.length && nums[hi] == 0) {
                hi++;
            }
            
            if (lo >= nums.length || hi >= nums.length) break;
            
            nums[lo] = nums[hi];
            nums[hi] = 0;
        }
    }
}
```


### ⭐ 다른 정답

```java
class Solution {
     public void moveZeroes(int[] nums) {
        int snowBallSize = 0; 
        for (int i = 0; i < nums.length; i++){
	        if (nums[i] == 0){
                snowBallSize++; 
            }
            else if (snowBallSize > 0) {
	            int t = nums[i];
	            nums[i] = 0;
	            nums[i-snowBallSize] = t;
            }
        }
    }
}
```


### 💡 다른 정답

```java
public void moveZeroes(int[] nums) {
    if (nums == null || nums.length == 0) return;        

    int insertPos = 0;
    for (int num: nums) {
        if (num != 0) nums[insertPos++] = num;
    }        

    while (insertPos < nums.length) {
        nums[insertPos++] = 0;
    }
}
```


## 📝 Sort Array By Parity(패리티로 어레이 정렬)

- 정수 배열 nums가 주어지면 배열의 시작 부분에 있는 모든 짝수 정수와 모든 홀수 정수를 이동합니다. 
- 해석 : 배열의 시작 부분에 짝수 정수들이 오고 그 이후에 홀수 정수를 배치하도록 이동시킨다.
- 이 조건을 만족하는 모든 배열을 반환합니다.

### Example 1 :

```text
Input: nums = [3,1,2,4]
Output: [2,4,3,1]
Explanation: 출력 [4,2,3,1], [2,4,1,3] 및 [4,2,1,3]도 허용됩니다.
```

### Example 2 :

```text
Input: nums = [0]
Output: [0]
```

### Constraints :

- `1 <= nums.length <= 5000`
- `0 <= nums[i] <= 5000`

---

### 🔍 정답

```java
class Solution {
    public int[] sortArrayByParity(int[] nums) {
        int lo = 0;
        int hi = nums.length - 1;
        
        while (true) {
            while (lo + 1 < nums.length && nums[lo] % 2 == 0) {
                lo++;
            }
            
            while (hi - 1 >= 0 && nums[hi] % 2 == 1) {
                hi--;
            }
            
            if (lo >= hi) break;
            
            int tmp = nums[lo];
            nums[lo] = nums[hi];
            nums[hi] = tmp;
        }
        
        return nums;
    }
}
```


### ⭐ 다른 정답

```java
public int[] sortArrayByParity(int[] A) {
        for (int i = 0, j = 0; j < A.length; j++)
            if (A[j] % 2 == 0) {
                int tmp = A[i];
                A[i++] = A[j];
                A[j] = tmp;;
            }
        return A;
    }
```