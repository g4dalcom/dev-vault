# 개념

여러 값이 주어졌을 때 k번째로 작은 값이나 큰 값을 찾을 때 유용한 검색 알고리즘이다.

퀵 정렬과 유사하게 pivot이라는 임의의 값을 기준으로 배열을 분할하는데 퀵 정렬이 양쪽으로 탐색하는 반면, 퀵 셀렉트는 한쪽으로만 탐색한다는 점에서 차이가 있다.

pivot값을 어떻게 선택하느냐에 따라 성능이 좌우되는데 이상적인 경우에는 O(N), 최악의 경우는 O(N^2)이 될 수 있다.


## 흐름

1. 피봇을 정한다.
2. 피봇과 비교해서 더 작은 값은 왼쪽으로, 큰 값은 오른쪽으로 보낸다.
3. 피봇의 인덱스가 k보다 작다면 오른쪽만 탐색, 피봇의 인덱스가 k보다 크다면 왼쪽만 탐색!


## 코드 구현

```java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        int start = 0;
        int end = nums.length-1;
        int target = nums.length - k;
        
        while (start < end) {
            int pivot = partition(nums, start, end);
            if (pivot < target) start = pivot + 1;
            else if (pivot > target) end = pivot - 1;
            else return nums[pivot];
        }
        
        return nums[start];
    }
    
    public int partition(int[] nums, int start, int end) {
        int pivot = start;
        
        while (start <= end) {
            while (start <= end && nums[start] <= nums[pivot]) start++;
            while (start <= end && nums[end] > nums[pivot]) end--;
            
            if (start <= end) {
                swap(nums, start, end);
            }
        }
        swap(nums, end, pivot);
        return end;
        
    }
    
    public void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```