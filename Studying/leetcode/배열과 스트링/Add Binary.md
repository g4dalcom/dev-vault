Given two binary strings `a` and `b`, return _their sum as a binary string_.

**Example 1:**

**Input:** a = "11", b = "1"
**Output:** "100"

**Example 2:**

**Input:** a = "1010", b = "1011"
**Output:** "10101"

**Constraints:**

-   `1 <= a.length, b.length <= 104`
-   `a` and `b` consist only of `'0'` or `'1'` characters.
-   Each string does not contain leading zeros except for the zero itself.

---

### 나의 풀이

```java
class Solution {
    public String addBinary(String a, String b) {
        Stack<Integer> stack = new Stack<>();
        
        int ap = a.length()-1;
        int bp = b.length()-1;
        
        int ceil = 0;
        while(true) {
            if (ap < 0) {
                while (bp >= 0) {
                    int cal = b.charAt(bp) - '0' + ceil;
                    stack.push(cal % 2);
                    ceil = cal >= 2 ? 1 : 0;
                    bp--;
                }
                if (ceil == 1) stack.push(ceil);
                break;
            } else if (bp < 0) {
                while (ap >= 0) {
                    int cal = a.charAt(ap) - '0' + ceil;
                    stack.push(cal % 2);
                    ceil = cal >= 2 ? 1 : 0;
                    ap--;
                }
                if (ceil == 1) stack.push(ceil);
                break;
            }
            
            int ia = a.charAt(ap) - '0';
            int ib = b.charAt(bp) - '0';
            
            int cal = ia + ib + ceil;
            
            if (cal >= 2) {
                stack.push(cal % 2);
                ceil = 1;
            } else {
                stack.push(cal);
                ceil = 0;
            }
            
            ap--;
            bp--;
        }
        
        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) {
            sb.append(stack.pop());
        }
        
        return sb.toString();
    }
}
```

### 리팩토링

```java
class Solution {
    public String addBinary(String a, String b) {
        StringBuilder sb = new StringBuilder();
        
        int ap = a.length()-1;
        int bp = b.length()-1;
        
        int ceil = 0;
        
        while (ap >= 0 || bp >= 0) {
            int sum = ceil;
            if (ap >= 0) {
                sum += a.charAt(ap) - '0';
                ap--;
            }
            if (bp >= 0) {
                sum += b.charAt(bp) - '0';
                bp--;
            }
            
            sb.append(sum >= 2 ? sum % 2 : sum);
            ceil = sum / 2;
        }
        
        if (ceil != 0) sb.append(ceil);
        
        return sb.reverse().toString();
    }
}
```