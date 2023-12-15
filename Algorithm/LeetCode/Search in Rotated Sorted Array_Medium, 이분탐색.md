# [문제링크](https://leetcode.com/problems/search-in-rotated-sorted-array/)

## 📝 문제

There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return _the index of_ `target` _if it is in_ `nums`_, or_ `-1` _if it is not in_ `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

**Input:** nums = [4,5,6,7,0,1,2], target = 0
**Output:** 4

**Example 2:**

**Input:** nums = [4,5,6,7,0,1,2], target = 3
**Output:** -1

**Example 3:**

**Input:** nums = [1], target = 0
**Output:** -1

**Constraints:**

- `1 <= nums.length <= 5000`
- `-104 <= nums[i] <= 104`
- All values of `nums` are **unique**.
- `nums` is an ascending array that is possibly rotated.
- `-104 <= target <= 104`

---

### 💡 풀이

- 문제 설명이 복잡하게 되어있는데 요약하면, 배열 안에 target 값의 인덱스를 반환하는 것입니다.

```java
import java.util.*;
import java.util.stream.Stream;

class Solution {
    public int search(int[] nums, int target) {
        return Arrays.stream(nums).boxed().collect(Collectors.toList()).indexOf(target);
    }
}
```

- 위와 같은 방식으로도 해결은 되지만 문제의 조건 중 시간복잡도 O(log n) 으로 해결하라는 것이 있기 때문에 출제 의도와는 맞지 않는 풀이입니다.
- log n 의 시간 복잡도로 해결을 하기 위해서는 nums 배열을 정렬하지 않고 그대로 사용해야 하고 탐색도 최소한으로 해야 합니다. 
- 이분 탐색을 쉽게 떠올릴 수 있는데 이분 탐색의 조건은 정렬된 리스트라는 것이기 때문에 그대로 적용할 수 없어서 까다로웠던 문제입니다.
- 우선 0 1 2 4 5 6 7 처럼 쭉 오름차순으로 정렬되어 있는 경우에는 이분 탐색을 그대로 적용하면 될 것이지만
- 7 0 1 2 4 5 6 처럼 mid 값이 2일 때 mid의 오른쪽은 오름차순이지만 왼쪽은 그렇지가 않습니다. 이러한 경우를 구분해서 탐색을 해주어야 합니다.
- 위 예시를 들었을 때 mid는 left보다 오른쪽에 위치하기 때문에 정렬된 리스트라면 nums\[mid\] >= nums\[left\] 를 항상 만족할 것입니다. 따라서 mid와 left를 비교해서 오름차순이 성립한다면 target값과 비교하는 이분 탐색을 자연스럽게 적용해주면 됩니다.
- 그러나 nums\[mid\] < nums\[left\] 라면 7 0 1 2 4 5 6 과 같은 모양일 것입니다. 이런 경우에는 오름차순으로 되어있는 오른쪽의 범위에 있는지 여부를 탐색하는 방식으로 구현할 수 있었습니다.

### 🔍 정답

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length-1;

        while (left <= right) {
            int mid = (left + right) / 2;
            
            if (nums[mid] == target) return mid;

            if (nums[left] <= nums[mid]) {
                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            } else {
                if (target > nums[mid] && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }

        return -1;
    }
}
```