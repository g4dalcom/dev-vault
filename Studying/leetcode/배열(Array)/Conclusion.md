## 📝 Height Checker(높이 검사기)

- 한 학교에서 모든 학생들의 연례 사진을 찍으려고 합니다. 학생들은 높이가 감소하지 않는 순서로 한 줄로 서도록 요청받습니다.
- 이 순서를 expected[i]가 줄에서 i번째 학생의 예상 높이인 예상 정수 배열로 표현하도록 합니다.
- 학생들이 서 있는 현재 순서를 나타내는 정수 배열 heights가 제공됩니다. 각 heights[i]는 줄에서 i번째 학생의 키입니다(인덱스 0).
- heights[i] != expected[i]인 인덱스의 수를 반환합니다.


### Example 1 :

```text
Input: heights = [1,1,4,2,1,3]
Output: 3
Explanation: 
heights:  [1,1,4,2,1,3]
expected: [1,1,1,2,3,4]
Indices 2, 4, and 5 do not match.
```

### Example 2 :

```text
Input: heights = [5,1,2,3,4]
Output: 5
Explanation:
heights:  [5,1,2,3,4]
expected: [1,2,3,4,5]
All indices do not match.
```


### Example 3 :

```text
Input: heights = [1,2,3,4,5]
Output: 0
Explanation:
heights:  [1,2,3,4,5]
expected: [1,2,3,4,5]
All indices match.
```

### Constraints :

- `1 <= heights.length <= 100`
- `1 <= heights[i] <= 100`

---

### 🔍 정답

```java
class Solution {
    public int heightChecker(int[] heights) {
        int[] expected = heights.clone();
        Arrays.sort(expected);
        
        int cnt = 0;
        for (int i = 0; i < heights.length; i++) {
            if (heights[i] != expected[i]) cnt++;
        }
        
        return cnt;
    }
}
```


## 📝 Third Maximum Number(세 번째 최대수)

- 정수 배열 nums가 주어지면 이 배열에서 세 번째 고유 최대 수를 반환합니다. 세 번째 최대값이 없으면 최대값을 반환합니다.

### Example 1 :

```text
Input: nums = [3,2,1]
Output: 1
Explanation:
The first distinct maximum is 3.
The second distinct maximum is 2.
The third distinct maximum is 1.
```

### Example 2 :

```text
Input: nums = [1,2]
Output: 2
Explanation:
The first distinct maximum is 2.
The second distinct maximum is 1.
The third distinct maximum does not exist, so the maximum (2) is returned instead.
```


### Example 3 :

```text
Input: nums = [2,2,3,1]
Output: 1
Explanation:
The first distinct maximum is 3.
The second distinct maximum is 2 (both 2's are counted together since they have the same value).
The third distinct maximum is 1.
```



### Constraints :

- `1 <= nums.length <= 104`
- `-231 <= nums[i] <= 231 - 1`

---

### 🔍 정답

```java
class Solution {
    public int thirdMax(int[] nums) {
        Arrays.sort(nums);
        
        int cnt = 2;
        int cur = nums[nums.length-1];
        for (int i = nums.length-1; i >= 0; i--) {
            if (nums[i] < cur) {
                cur = nums[i];
                cnt--;
            }
            
            if (cnt == 0) return cur;
        }
        
        return nums[nums.length-1];
    }
}
```



## 📝 Find All Numbers Disappeared in an Array(배열에서 사라진 모든 숫자 찾기)

- nums[i]가 [1, n] 범위에 있는 n 정수의 배열 nums가 주어지면 [1, n] 범위에서 nums에 나타나지 않는 모든 정수의 배열을 반환합니다.

### Example 1 :

```text
Input: nums = [4,3,2,7,8,2,3,1]
Output: [5,6]
```

### Example 2 :

```text
Input: nums = [1,1]
Output: [2]
```

### Constraints :

- `n == nums.length`
- `1 <= n <= 105`
- `1 <= nums[i] <= n`

---

### 🔍 정답

```java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        ArrayList<Integer> result = new ArrayList<>();
        int[] check = new int[nums.length+1];
        
        for (int num : nums) {
            check[num]++;
        }
        
        for (int i = 1; i <= nums.length; i++) {
            if (check[i] == 0) result.add(i);
        }
        
        return result;
    }
}
```


### ⭐ 다른 정답

```java

```