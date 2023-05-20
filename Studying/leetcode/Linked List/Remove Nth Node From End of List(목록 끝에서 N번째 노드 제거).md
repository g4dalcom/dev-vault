- 연결된 목록의 헤드가 주어지면 목록 끝에서 n번째 노드를 제거하고 헤드를 반환합니다.

![](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

### Example 1 :

```text
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

### Example 2 :

```text
Input: head = [1], n = 1
Output: []
```

### Example 3 :

```text
Input: head = [1,2], n = 1
Output: [1]
```


### Constraints :

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

---

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        ListNode left = dummy;
        ListNode right = dummy;
        left.next = head;
        
        while (n-- >= 0) {
            right = right.next;
        }
        
        while (right != null) {
            right = right.next;
            left = left.next;
        }
        
        left.next = left.next.next;
        
        return dummy.next;
    }
}
```
- 예를 들어 head = [1, 2, 3, 4, 5, 6, 7] 이고 N=3 이라면,
- 뒤에서부터 세 번째인 5가 타겟이다.

1. 0번 dummy node에서 시작!
	- head = [1, 2, 3] 이고 N = 3이라면 1이 제거되어야 하는데 우리는 N+1 만큼의 거리를 둘 필요가 있다. 따라서 dummy node의 0번을 주고 시작해야 한다!
2. 두 개의 포인터를 N+1 만큼 거리를 둔다.
	- left = 0, right = 4
3. 오른쪽 포인터가 null이 될 때까지 두 개의 포인터를 하나씩 움직인다.
	- (0, 4) -> (1, 5) -> (2, 6) -> (3, 7) -> (4, null)
	- left = 4, right = null
4. 그러면 left가 타겟인(5) 바로 전에 위치한다. 타겟을 삭제하려면 간단하게 연결만 끊어주면 되므로 left.next = left.next.next를 해준다.
5. 0번 노드를 제거하면서 바뀐 리스트를 반환하면 된다!