## 📝 Max Consecutive Ones(최대 연속 1)

### Example 1 :

```text
Input: nums = [1,1,0,1,1,1]
Output: 3
Explanation: 처음 두 자리 또는 마지막 세 자리는 연속된 1입니다. 연속 1의 최대 수는 3입니다.
```

### Example 1 :

```text
Input: nums = [1,0,1,1,0,1]
Output: 2
```


### Constraints :

- `1 <= nums.length <= 105`
- `nums[i]` is either `0` or `1`.

---

### 🔍 정답

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int cnt = 0;
        int max = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 1) cnt++;
            else {
                max = Math.max(max, cnt);
                cnt = 0;
            }
            max = Math.max(max, cnt);
        }
        
        return max;
    }
}
```

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int cnt = 0;
        int max = 0;
        
        for (int num : nums) {
            if (num == 1) {
                cnt++;
                max = Math.max(max, cnt);
            } else cnt = 0;
        }
        
        return max;
    }
}
```


### ⭐ 다른 정답

```java
public int findMaxConsecutiveOnes(int[] nums) {
        int maxHere = 0, max = 0;
        for (int n : nums)
            max = Math.max(max, maxHere = n == 0 ? 0 : maxHere + 1);
        return max; 
    } 
```

```java
110111
^ maxHere = 1

110111
.^ maxHere = 2

110111
..^ maxHere = 0

110111
...^ maxHere = 1

110111
....^ maxHere = 2

110111
.....^ maxHere = 3
```



## 📝 Find Numbers with Even Number of Digits(자릿수가 짝수인 숫자 찾기)

### Example 1 :

```text
Input: nums = [12,345,2,6,7896]
Output: 2
Explanation: 
12 contains 2 digits (even number of digits). 
345 contains 3 digits (odd number of digits). 
2 contains 1 digit (odd number of digits). 
6 contains 1 digit (odd number of digits). 
7896 contains 4 digits (even number of digits). 
Therefore only 12 and 7896 contain an even number of digits.
```

### Example 2 :

```text
Input: nums = [555,901,482,1771]
Output: 1 
Explanation: 
Only 1771 contains an even number of digits.
```

### Constraints :

- `1 <= nums.length <= 500`
- `1 <= nums[i] <= 105`

---

### 🔍 정답

```java
class Solution {
    public int findNumbers(int[] nums) {
        int cnt = 0;
        for (int i = 0; i < nums.length; i++) {
            String[] str = String.valueOf(nums[i]).split("");
            if (isEven(str.length)) cnt++;
        }
        
        return cnt;
        
    }
    
    public boolean isEven(int num) {
        return num % 2 == 0;
    }
}
```


### ⭐ 다른 정답

```java
public int findNumbers(int[] nums) {
	int sol = 0;
	for(int n : nums)
		if(getNumberOfDigits(n) % 2 == 0)
			sol++;
	return sol;                             
}

public int getNumberOfDigits(int num) {
	int count = 1;
	while((num /= 10) != 0)
		count++;
	return count;
}
```


### ⭐ 다른 정답

```java
public int findNumbers(int[] nums) {
	return Arrays.stream(nums).map(i -> 1 - Integer.toString(i).length() % 2).sum();
}
```


## 📝 Squares of a Sorted Array(정렬된 배열의 제곱)

### Example 1 :

```text
Input: nums = [-4,-1,0,3,10]
Output: [0,1,9,16,100]
Explanation: 제곱 후 배열은 [16,1,0,9,100]이 됩니다. 정렬하면 [0,1,9,16,100]이 됩니다.
```

### Example 2 :

```text
Input: nums = [-7,-3,2,3,11]
Output: [4,9,9,49,121]
```

### Constraints :

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` is sorted in **non-decreasing** order.

---

### 🔍 정답

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        
        for (int i = 0; i < nums.length; i++) {
            pq.offer((int) Math.pow(nums[i], 2));
        }
        
        int[] result = new int[nums.length];
        for (int i = 0; i < nums.length; i++) {
            result[i] = pq.poll();
        }
        
        return result;
    }
}
```


### ⭐ 다른 정답

```java
class Solution {
    public int[] sortedSquares(int[] nums) {
        int[] returnArr = new int[nums.length];
        int i = 0; 
        int j = nums.length - 1;
        int index = nums.length - 1;
        while (i <= j) {
            if (Math.abs(nums[i]) > Math.abs(nums[j])) {
                returnArr[index] = nums[i] * nums[i];
                i++;
            } else {
                returnArr[index] = nums[j] * nums[j];
                j--;
            }
            index--;
        }
        return returnArr;
    }
}
```