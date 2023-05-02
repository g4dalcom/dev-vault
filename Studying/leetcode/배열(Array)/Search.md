## 📝 Check If N and Its Double Exist(N과 그 2배수가 존재하는지 확인)

- 정수 배열 arr이 주어지면 두 개의 인덱스 i와 j가 존재하는지 확인합니다.
	- `i != j`
	- `0 <= i, j < arr.length`
	- `arr[i] == 2 * arr[j]`

### Example 1 :

```text
Input: arr = [10,2,5,3]
Output: true
Explanation: For i = 0 and j = 2, arr[i] == 10 == 2 * 5 == 2 * arr[j]
```

### Example 2 :

```text
Input: arr = [3,1,7,11]
Output: false
Explanation: There is no i and j that satisfy the conditions.
```

### Constraints :

- `2 <= arr.length <= 500`
- `-103 <= arr[i] <= 103`

---

### 🔍 정답

```java
class Solution {
    public boolean checkIfExist(int[] arr) {
        Arrays.sort(arr);
        
        for (int i = 0; i < arr.length; i++) {
            for (int j = i+1; j < arr.length; j++) {
                if (arr[i] < 0 && arr[i] % 2 == 0) {
                    if (arr[i] / 2 == arr[j]) return true;
                } else if (arr[i] >= 0) {
                    if (arr[i] * 2 == arr[j]) return true;
                }
            }
        }
        return false;
    }
}
```


### ⭐ 다른 정답(HashSet)

```java
public boolean checkIfExist(int[] arr) {
	HashSet<Integer> set = new HashSet<>();
	for(int a : arr) {
		if(set.contains(a*2) || (a%2 == 0 && set.contains(a/2))) return true;
		set.add(a);
	}
	return false;
}
```

### ⭐ 다른 정답(binary search)

```java
class Solution {
    public boolean checkIfExist(int[] arr) {
        Arrays.sort(arr);
        for (int i = 0; i < arr.length; i++) {
            int target = 2 * arr[i];
            int lo = 0, hi = arr.length - 1;
            while (lo <= hi) {
                int mid = lo + (hi - lo) / 2;
                if (arr[mid] == target && mid != i) 
                    return true;
                if (arr[mid] < target) 
                    lo = mid + 1;
                else 
                    hi = mid - 1;
            }
        }

        return false;
    }
}
```


## 📝 Valid Mountain Array(유효한 마운틴 어레이)

- 정수 배열 arr이 주어지면 유효한 마운틴 배열인 경우에만 true를 반환합니다. 다음과 같은 경우에만 arr이 마운틴 배열임을 상기하십시오

![](https://assets.leetcode.com/uploads/2019/10/20/hint_valid_mountain_array.png)

- `arr.length >= 3`
- There exists some `i` with `0 < i < arr.length - 1` such that:
    - `arr[0] < arr[1] < ... < arr[i - 1] < arr[i]`
    - `arr[i] > arr[i + 1] > ... > arr[arr.length - 1]`


### Example 1 :

```text
Input: arr = [2,1]
Output: false
```

### Example 2 :

```text
Input: arr = [3,5,5]
Output: false
```

### Example 3 :

```text
Input: arr = [0,3,2,1]
Output: true
```

### Constraints :

- `1 <= arr.length <= 104`
- `0 <= arr[i] <= 104`

---

### 🔍 정답

```java
class Solution {
    public boolean validMountainArray(int[] arr) {
        if (arr.length < 3) return false;
        
        int cur1 = 0;
        for (int i = 0; i < arr.length-1; i++) {
            if (arr[i+1] > arr[cur1]) {
                cur1 = i+1;
            } else break;
        }
        
        int cur2 = arr.length-1;
        for (int i = arr.length-1; i >= 1; i--) {
            if (arr[i-1] > arr[cur2]) {
                cur2 = i-1;
            } else break;
        }
        
        if (cur1 == 0 || cur2 == arr.length-1) return false;
        
        return cur1 == cur2;
    }
}
```


### ⭐ 다른 정답

```java
class Solution {
    public boolean validMountainArray(int[] arr) {
        if(arr.length < 3) return false;
        int l = 0;
        int r = arr.length - 1;
        while(l + 1 < arr.length - 1 && arr[l] < arr[l + 1]) l++;
        while(r - 1 > 0 && arr[r] < arr[r - 1]) r--;
        return l == r;
    }
}
```