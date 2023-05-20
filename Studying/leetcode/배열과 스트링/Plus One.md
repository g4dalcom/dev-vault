You are given a large integer represented as an integer array digits, where each digits[i] is the ith digit of the integer. The digits are ordered from most significant to least significant in left-to-right order. The large integer does not contain any leading 0's.

Increment the large integer by one and return the resulting array of digits.

 

Example 1:

Input: digits = [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.
Incrementing by one gives 123 + 1 = 124.
Thus, the result should be [1,2,4].
Example 2:

Input: digits = [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.
Incrementing by one gives 4321 + 1 = 4322.
Thus, the result should be [4,3,2,2].
Example 3:

Input: digits = [9]
Output: [1,0]
Explanation: The array represents the integer 9.
Incrementing by one gives 9 + 1 = 10.
Thus, the result should be [1,0].
 

Constraints:

1 <= digits.length <= 100
0 <= digits[i] <= 9
digits does not contain any leading 0's.

---

```java
class Solution {
    public int[] plusOne(int[] digits) {
        int pointer = digits.length - 1;
        
        while (digits[pointer] == 9) {            
 
            if (pointer <= 0) {
                int[] newArr = new int[digits.length+1];
                newArr[0] = 1;
                
                for (int i = 1; i < newArr.length; i++) {
                    newArr[i] = digits[i-1];
                }
                newArr[pointer+1] = 0;
                return newArr;
            }            
            
            digits[pointer] = 0;
            pointer--;
        }
        
        digits[pointer]++;
        
        return digits;
    }
}
```


### 다른 정답

```java
public int[] plusOne(int[] digits) {
        
    int n = digits.length;
    for(int i=n-1; i>=0; i--) {
        if(digits[i] < 9) {
            digits[i]++;
            return digits;
        }
        
        digits[i] = 0;
    }
    
    int[] newNumber = new int [n+1];
    newNumber[0] = 1;
    
    return newNumber;
}
```
- 정말 심플하면서 대단한 풀이인 것 같다.
- 생각해보면 [9] 와 같은 입력이 들어와서 [1, 0] 처럼 배열의 길이를 늘려야 하는 때는 모든 앞자리 수들이 9일 때밖에 없다.
- [9, 8] 만 해도 9, 9 가 되기 때문에 배열의 길이는 늘어나지 않는다.
- 나는 맨 앞자리가 9면 배열이 늘어날 거라는 단순한 생각을 가지고 풀어서 풀이가 복잡해진 것 같다 ㅠㅠ
- 배열을 선언하면 기본값으로 0이 주어지므로 0번 인덱스만 1로 선언해주면 되는 것이었다....