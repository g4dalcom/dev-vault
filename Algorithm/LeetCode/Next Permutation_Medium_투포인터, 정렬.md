# [문제링크](https://leetcode.com/problems/next-permutation/)

## 📝 문제

A **permutation** of an array of integers is an arrangement of its members into a sequence or linear order.

- For example, for `arr = [1,2,3]`, the following are all the permutations of `arr`: `[1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1]`.

The **next permutation** of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the **next permutation** of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

- For example, the next permutation of `arr = [1,2,3]` is `[1,3,2]`.
- Similarly, the next permutation of `arr = [2,3,1]` is `[3,1,2]`.
- While the next permutation of `arr = [3,2,1]` is `[1,2,3]` because `[3,2,1]` does not have a lexicographical larger rearrangement.

Given an array of integers `nums`, _find the next permutation of_ `nums`.

The replacement must be **[in place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [1,3,2]

**Example 2:**

**Input:** nums = [3,2,1]
**Output:** [1,2,3]

**Example 3:**

**Input:** nums = [1,1,5]
**Output:** [1,5,1]

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 100`

---

### 💡 풀이

- 정수로 이루어진 배열이 있을 때, 이 요소들을 사전순 기준으로 다음에 올 정수 배열로 재배열해야 하는 문제입니다.
- \[1, 2, 3\] 다음으로 오는 정수배열은 1, 2, 3을 가지고 만들 수 있는 바로 다음의 큰 수이므로 \[1, 3, 2\]가 되고 그 다음은 \[2, 1, 3\] 이 되는 것입니다.
- 우리는 132 다음이 213이라고 직관적으로 아는 것 같지만 알게모르게 연산 과정을 거쳐서 213이라고 판단을 하게 됩니다. 이 연산 과정을 코드로 구현하면 되겠죠?!
- 숫자를 하나 더 늘려서 1234 라는 수를 놓고 보면, 1234 다음이 1243 이 된다는 것은 어떻게 안 것일까요?
- 가장 먼저 우리는 마지막 두 수를 비교해봅니다. 3과 4를 비교해서 3이 더 작다면 두 수를 스왑하는 것이죠. 우리는 이 연산을 아주 자연스럽게 하기 때문에 연산을 했다고 생각하지도 못했을 것입니다.
- 그러면 1243 다음 수인 1324 는 어떤 식의 연산을 거치게 되는 것일까요?
- 처음 두 수인 4, 3을 비교했을 때 스왑하면 수가 더 작아진다는 것을 알고 다른 수를 비교하게 됩니다. 그 때 우리는 2, 4, 3 세 수를 쳐다보게 되죠! 여기서 2 자리에 3이 오게 되는데 이 때 우리는 4와 3중 2보다는 크지만 둘 중 더 작은 수를 택합니다.
- 1342 로 바뀐 수에서 가능한 더 작은 수를 만들어야 하므로 우리는 다시 4 와 2를 스왑해서 1324를 만들게 되죠.
- 우리가 자연스럽게 하는 이 연산에는 일정한 규칙이 있는데요!

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbq3nU9%2FbtsBJcqFuvp%2FkGBiSFpZJ5e7xRxiBpf5h0%2Fimg.png)

- 먼저 배열의 끝에서부터 두 수를 비교합니다. left가 더 작을 때는 두 수를 스왑하면 바로 한 단계 더 큰 수가 되겠지만 left가 더 작다면 스왑을 해서는 안 되겠죠?!

```java
int left = nums.length - 2;
int right = nums.length - 1;

while (left > 0 && nums[left] >= nums[right]) {
	left--;
	right--;
}
```

- 따라서 위와 같은 반복문을 통해 left가 더 작은 경우를 찾습니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcPoRuB%2FbtsBMUpKYpn%2F84iyDdXKEjkwO9oJYcw6u0%2Fimg.png)

- 그리고 다시 배열의 끝에 target이라는 포인터를 두고 탐색을 합니다. 이 수가 스왑 대상인데 만약 target이 left보다 작으면 스왑했을 때 더 작은 수가 되니까 스왑하면 안 되겠죠?!

```java
int target = nums.length-1;
while (target > 0 && nums[left] >= nums[target]) {
	target--;
}

swap(left, target, nums);
```

- 따라서 위 반복문을 통해 target 값을 구해준 뒤 left와 target을 스왑합니다.
- 이 때 처음에 구한 right와 스왑하는 것이 아니라 target을 따로 구하는 이유는, 바로 다음 큰 수를 구해야 하기 때문입니다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbza6M7%2FbtsBMPWfPJs%2FmuK6JAk1s5K4aq1VaGnw11%2Fimg.png)

- 스왑을 한 뒤에는 바뀐 left값을 제외하고 뒷 부분을 정렬해줌으로써 바로 다음 큰 수로 만들어줄 수 있습니다.

```java
if (left == 0 && nums[left] >= nums[right]) right--;
```

- 문제에서 3 2 1 처럼 다음 큰 수가 존재하지 않는 경우에는 1 2 3 처럼 가장 작은 수로 되돌아가야 한다고 정의하고 있습니다.
- 만약 스왑을 할 대상이 없다면 우리는 맨 마지막 로직인 정렬작업만 해주면 되는데요! left와 right를 동시에 -- 해주기 때문에 left > 0 이라는 조건을 걸었는데 이 때문에 0번 인덱스와 1번 인덱스를 비교하지 않는 문제가 있습니다.
- 그래서 위 로직을 통해 0, 1번 인덱스까지 비교를 하고 그래도 스왑할 대상이 없다면 right = 0이 되므로 0번 인덱스부터 차례로 정렬을 하게 됩니다.

### 🔍 정답

```java
class Solution {
    public void nextPermutation(int[] nums) {
        if (nums.length > 1) {
            int left = nums.length - 2;
            int right = nums.length - 1;

            while (left > 0 && nums[left] >= nums[right]) {
                left--;
                right--;
            }

            if (left == 0 && nums[left] >= nums[right]) right--;

            int target = nums.length-1;
            while (target > 0 && nums[left] >= nums[target]) {
                target--;
            }

            swap(left, target, nums);

            for (int i = right; i < nums.length; i++) {
                int min_index = i;
                for (int j = i+1; j < nums.length; j++) {
                    if (nums[j] < nums[min_index]) {
                        min_index = j;
                    }
                }
                swap(i, min_index, nums);
            }
        }
    }

    public void swap(int left, int right, int[] nums) {
        int temp = nums[left];
        nums[left] = nums[right];
        nums[right] = temp;
    }
}
```