# [문제링크](https://leetcode.com/problems/add-two-numbers/)

## 📝 문제

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/02/addtwonumber1.jpg)

**Input:** l1 = [2,4,3], l2 = [5,6,4]
**Output:** [7,0,8]
**Explanation:** 342 + 465 = 807.

**Example 2:**

**Input:** l1 = [0], l2 = [0]
**Output:** [0]

**Example 3:**

**Input:** l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
**Output:** [8,9,9,9,0,0,0,1]

**Constraints:**

- The number of nodes in each linked list is in the range `[1, 100]`.
- `0 <= Node.val <= 9`
- It is guaranteed that the list represents a number that does not have leading zeros.

---

### 💡 풀이

- 2개의 단일연결리스트가 주어지고 두 연결리스트들을 연산하는 문제입니다.
- 연결리스트는 일의 자리부터 주어지며 연산의 결과 또한 일의 자리부터 연결리스트에 집어넣어야 합니다.
- 연결 리스트의 길이는 최대 100까지 존재할 수 있기 때문에 한 번의 연산만으로 계산할 수는 없고 일의 자리부터 차근차근 연산을 해야 합니다.
- 일의 자리부터 덧셈을 하게 되면 올림(carry)이 발생할 수 있다는 것과 l1과 l2의 길이가 같지 않다는 점을 생각해야 했습니다.

### 🔍 정답

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode result = new ListNode();
        ListNode node = result;

        int sum = 0;
        while (l1 != null || l2 != null || sum > 0) {
            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }

            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }

            node.next = new ListNode(sum % 10);
            sum /= 10;

            node = node.next;
        }
        
        return result.next;
    }
}
```