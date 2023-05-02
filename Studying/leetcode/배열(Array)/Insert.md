## 📝 Duplicate Zeros(중복 0)

- 고정 길이 정수 배열 arr이 주어지면 각 0을 복제하고 나머지 요소를 오른쪽으로 이동합니다.
- 원래 배열의 길이를 초과하는 요소는 기록되지 않습니다. 입력 배열에 대해 위의 수정을 수행하고 아무 것도 반환하지 마십시오.

### Example 1 :

```text
Input: arr = [1,0,2,3,0,4,5,0]
Output: [1,0,0,2,3,0,0,4]
Explanation: After calling your function, the input array is modified to: [1,0,0,2,3,0,0,4]
```

### Example 2 :

```text
Input: arr = [1,2,3]
Output: [1,2,3]
Explanation: After calling your function, the input array is modified to: [1,2,3]
```

### Constraints :

- `1 <= arr.length <= 104`
- `0 <= arr[i] <= 9`

---

### 🔍 정답

```java
class Solution {
    public void duplicateZeros(int[] arr) {
        String str = "";
        for (int i = 0; i < arr.length; i++) {
            str += String.valueOf(arr[i]);
        }
        
        str = str.replace("0", "00");
        
        for (int i = 0; i < arr.length; i++) {
            char tmp = str.charAt(i);
            arr[i] = tmp - '0';
        }
    }
}
```


### ⭐ 다른 정답

```java
class Solution {
    public void duplicateZeros(int[] arr) {
        for(int i = 0; i < arr.length; i++){
            if(arr[i] == 0){
                shift(arr,i+1);
                if(i != arr.length-1){
                arr[i+1] = 0;
                    i++;
                }
            }
            }
        }
    public static void shift(int[] arr, int index){
        for(int i = arr.length-1; i > index; i--){
            arr[i] = arr[i-1];
        }
    }
}
```


### ⭐ 다른 정답

```java
  public void duplicateZeros(int[] A) {
    
    int n = A.length, count = 0;
    
    for (int num : A) if (num == 0) count++;
    int i = n - 1;
    int write = n + count - 1;
    
    while (i >= 0 && write >= 0)  {
      
      if (A[i] != 0) { // Non-zero, just write it in
        if (write < n) A[write] = A[i];
        
      } else { // Zero found, write it in twice if we can
        if (write < n) A[write] = A[i];
        write--;
        if (write < n) A[write] = A[i];
      }
      
      i--;
      write--;
    }
  }
```


## 📝  Merge Sorted Array(정렬된 배열 병합)

- 감소하지 않는 순서로 정렬된 두 개의 정수 배열 nums1 및 nums2와 각각 nums1 및 nums2의 요소 수를 나타내는 두 개의 정수 m 및 n이 제공됩니다.
- nums1과 nums2를 감소하지 않는 순서로 정렬된 단일 배열로 병합합니다.
- 최종 정렬된 배열은 함수에 의해 반환되지 않고 대신 배열 nums1 내부에 저장되어야 합니다.
- 이를 수용하기 위해 nums1의 길이는 m + n입니다. 여기서 첫 번째 m 요소는 병합해야 하는 요소를 나타내고 마지막 n 요소는 0으로 설정되므로 무시해야 합니다. nums2의 길이는 n입니다.

### Example 1 :

```text
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: 병합할 배열은 [1,2,3]과 [2,5,6]입니다. 병합 결과는 [1,2,2,3,5,6]이며 밑줄이 그어진 요소는 nums1에서 나옵니다.
```

### Example 2 :

```text
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].
```

### Example 3 :

```text
Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.
```

### Constraints :

- `nums1.length == m + n`
- `nums2.length == n`
- `0 <= m, n <= 200`
- `1 <= m + n <= 200`
- `-109 <= nums1[i], nums2[j] <= 109`

---

### 🔍 정답

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        
        int idx = m;
        
        for (int i = 0; i < n; i++) {
            nums1[idx++] = nums2[i];
        }
        
        Arrays.sort(nums1);
    }
}
```


### ⭐ 다른 정답

```java
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int p1 = m-1 , p2 = n-1 ,i = m+n-1;
        while(p2 >=0 ){
            if(p1 >=0 && nums1[p1] > nums2[p2]){
                nums1[i--] = nums1[p1--];
            } 
            else{
                nums1[i--] = nums2[p2--];
            }
        }
       }
}
```

![image](https://assets.leetcode.com/users/images/7b67174e-1593-4897-8ba7-b674f0a8814c_1620047705.8573153.png)