# [문제링크]()

## 📝 문제

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example 1:**

**Input:** nums = [-1,0,1,2,-1,-4]
**Output:** [[-1,-1,2],[-1,0,1]]
**Explanation:** 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.

**Example 2:**

**Input:** nums = [0,1,1]
**Output:** []
**Explanation:** The only possible triplet does not sum up to 0.

**Example 3:**

**Input:** nums = [0,0,0]
**Output:** [[0,0,0]]
**Explanation:** The only possible triplet sums up to 0.

**Constraints:**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`

---

### 💡 풀이

- 두 수를 더해서 어떠한 target 값을 구하는 문제라면 input을 정렬한 뒤 양 끝에 포인터를 두고 target값보다 현재 sum값이 작으면 left를 늘리고 크다면 right를 줄이는 방식으로 반복문 한 번으로 찾을 수가 있습니다.
- 이 문제는 세 수를 더하는 문제인데 두 수를 더하는 것과 크게 다르지 않다고 생각하고 접근하였습니다.
- 우선 input을 정렬한 뒤 기준값 하나를 잡고 기준값 + 1 인덱스를 left, 배열의 끝을 right로 두고 투포인팅을 수행하는 방식입니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    static Set<List<Integer>> set;

    public List<List<Integer>> threeSum(int[] nums) {
        set = new HashSet<>();
        Arrays.sort(nums);
        for (int i = 0; i < nums.length-2; i++) {
            calculate(i, nums);
        }

        return new ArrayList<>(set);
    }

    public void calculate(int start, int[] nums) {
        int left = start + 1;
        int right = nums.length - 1;

        while (left < right) {
            int value = nums[start] + nums[left] + nums[right];

            if (value == 0) {
                List<Integer> list = new ArrayList<>();
                list.addAll(Arrays.asList(nums[start], nums[left], nums[right]));
                set.add(list);
                left++;
                right--;
            } else if (value < 0) {
                left++;
            } else {
                right--;
            }
        }
    }
}
```

- 다른 분들의 풀이도 저와 크게 다르지 않았는데 좀 더 개선할 부분이 있었습니다.
- 예를 들어 \[-1, -1, -1, 0, 0, 1, 1, 2\] 라는 배열이 있을 때 0번 인덱스를 기준으로 해서 \[-1, 0, 1\] 을 구한 뒤 다시 1번 인덱스를 기준으로 할 때 똑같이 \[-1, 0, 1\] 을 구하게 됩니다. 물론 HashSet 으로 인해 중복된 값은 들어가지 않겠지만 불필요한 탐색을 하게 되는 것이죠...
- left와 right 값도 마찬가지입니다. 기준값 -1에서 left 가 0이었다면 left를 0으로 한 탐색은 필요가 없어지는 것이죠!

```java
for (int i = 0; i < nums.length-2; i++) {
	if (i > 0 && nums[i] == nums[i-1]) continue;
	calculate(i, nums);
}

						.
						.
						.

if (value == 0) {
	List<Integer> list = new ArrayList<>();
	list.addAll(Arrays.asList(nums[start], nums[left], nums[right]));
	set.add(list);

	while (left < right && nums[left] == nums[left + 1]) left++;
	while (left < right && nums[right] == nums[right - 1]) right--;

	left++;
	right--;
```

- 위와 같이 중복을 제거하는 로직을 추가하면 런타임 시간이 기존 283ms 에서 34ms 로 1/9이 됩니다!