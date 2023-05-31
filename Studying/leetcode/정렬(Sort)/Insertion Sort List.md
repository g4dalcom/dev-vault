Given the `head` of a singly linked list, sort the list using **insertion sort**, and return _the sorted list's head_.

The steps of the **insertion sort** algorithm:

1. Insertion sort iterates, consuming one input element each repetition and growing a sorted output list.
2. At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list and inserts it there.
3. It repeats until no input elements remain.

The following is a graphical example of the insertion sort algorithm. The partially sorted list (black) initially contains only the first element in the list. One element (red) is removed from the input data and inserted in-place into the sorted list with each iteration.

![](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/04/sort1linked-list.jpg)

**Input:** head = [4,2,1,3]
**Output:** [1,2,3,4]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/04/sort2linked-list.jpg)

**Input:** head = [-1,5,3,4,0]
**Output:** [-1,0,3,4,5]

**Constraints:**

- The number of nodes in the list is in the range `[1, 5000]`.
- `-5000 <= Node.val <= 5000`

---

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
    public ListNode insertionSortList(ListNode head) {
        ListNode dummy = new ListNode(Integer.MIN_VALUE);
        ListNode cur;
        
        while (head != null) {
            ListNode newList = head.next;
            
            cur = dummy;
         
            while (cur != null && cur.next != null && cur.next.val < head.val) {
                cur = cur.next;
            }
            
            head.next = cur.next;
            cur.next = head;
            head = newList;
        }
        
        return dummy.next;
    }
}
```

MIN_VALUE는 편의상 0으로 둔다.
cur은 head에서 하나씩 꺼내서 정렬하는 리스트이고
head는 input 리스트이자 앞에서부터 하나씩 요소가 줄어들게 된다.

저 두 리스트를 비교해가며 정렬한다고 생각하면 된다!

### 예시

4 2 1 3이 주어졌을 때,

##### 초기값
head = 4 2 1 3
cur = 0 (dummy)

첫 번째 루프에서는 내부 while문을 cur.next가 null이기 때문에 건너뛰게 된다.

cur = 0 4
head = 2 1 3

결과를 보면 cur.next 가 head의 맨 앞자리가 되고 head는 앞에서부터 하나씩 줄어들게 된다.

두 번째 루프부터는 cur.next부터 head의 value와 비교하며 head의 value가 더 작다면 해당 위치에 head를 넣어주는 것이다!

cur = 0 2 4
head = 1 3

cur = 0 1 2 4
head = 3

cur = 0 1 2 3 4
head = null
