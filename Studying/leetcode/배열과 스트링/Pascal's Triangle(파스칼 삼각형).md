Given an integer numRows, return the first numRows of Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:

Example 1:

Input: numRows = 5
Output: \[\[1],\[1,1\],\[1,2,1\],\[1,3,3,1\],\[1,4,6,4,1\]\]
Example 2:

Input: numRows = 1
Output: \[\[1\]\]
 

Constraints:

1 <= numRows <= 30

---

```java
class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> result = new ArrayList<>();

        for (int i = 0; i < numRows; i++) {
            result.add(new ArrayList<>());
        }

        for (int i = 0; i < numRows; i++) {
            for (int j = 0; j < i+1; j++) {
                if (j == 0 || j == i) {
                    result.get(i).add(1);
                } else {
                    int left = result.get(i-1).get(j-1);
                    int right = result.get(i-1).get(j);
                    result.get(i).add(left + right);
                }
            }
        }
        return result;
    }
}
```