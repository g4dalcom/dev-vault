Given an integer array `nums` of `2n` integers, group these integers into `n` pairs `(a1, b1), (a2, b2), ..., (an, bn)` such that the sum of `min(ai, bi)` for all `i` is **maximized**. Return _the maximized sum_.

**Example 1:**

**Input:** nums = [1,4,3,2]
**Output:** 4
**Explanation:** All possible pairings (ignoring the ordering of elements) are:
1. (1, 4), (2, 3) -> min(1, 4) + min(2, 3) = 1 + 2 = 3
2. (1, 3), (2, 4) -> min(1, 3) + min(2, 4) = 1 + 2 = 3
3. (1, 2), (3, 4) -> min(1, 2) + min(3, 4) = 1 + 3 = 4
So the maximum possible sum is 4.

**Example 2:**

**Input:** nums = [6,2,6,5,1,2]
**Output:** 9
**Explanation:** The optimal pairing is (2, 1), (2, 5), (6, 6). min(2, 1) + min(2, 5) + min(6, 6) = 1 + 2 + 6 = 9.

**Constraints:**

-   `1 <= n <= 104`
-   `nums.length == 2 * n`
-   `-104 <= nums[i] <= 104`

---

```java
class Solution {
    public int arrayPairSum(int[] nums) {
        Arrays.sort(nums);
        
        int sum = 0;
        for (int i = 0; i < nums.length; i+=2) {
            sum += Math.min(nums[i], nums[i+1]);
        }
        
        return sum;
    }
}
```

```java
class Solution {
    public int arrayPairSum(int[] nums) {
        int[] counting = new int[20001];
        
        for (int i = 0; i < nums.length; i++) {
            counting[nums[i] + 10000]++;
        }
        
        int sum = 0;
        boolean odd = true;
        for (int i = 0; i < counting.length; i++) {
            while (counting[i]-- > 0) {
                if (odd) {
                    sum += i - 10000;
                }               
                odd = !odd;
            }
        }        
        return sum;
    }
}
```

카운팅 배열을 이용해서 해당 값에 대응하는 인덱스를 늘려줌으로써 정렬되게끔 한다.

예를 들어서 \[6, 2, 6, 5, 1, 2\] 는 새로운 배열에서 \[0, 1, 2, 0, 0, 2\] 가 되고 카운팅 배열에서 값이 0보다 크다면 원본 배열에서 수가 존재하는 것이므로 작은 수부터 얻을 수 있게 된다.

이 문제는 수의 범위가 -10,000 <= x <= 10,000 이기 때문에 배열 인덱스로 지정해주기 위해 +10,000을 해주는 것이고 

둘씩 짝지었을 때 더 작은 수들의 합계를 구해야 하는 것이므로 홀수번째 수만 더해주면 되기 때문에 boolean 값을 이용하는 것이다!