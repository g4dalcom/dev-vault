# [문제링크](https://leetcode.com/problems/swap-nodes-in-pairs/)

## 📝 문제

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

**Input:** head = [1,2,3,4]
**Output:** [2,1,4,3]

**Example 2:**

**Input:** head = []
**Output:** []

**Example 3:**

**Input:** head = [1]
**Output:** [1]

**Constraints:**

- The number of nodes in the list is in the range `[0, 100]`.
- `0 <= Node.val <= 100`

---

### 💡 풀이

- 간단해보이지만 막상 구현하려니 굉장히 까다로웠던 문제입니다.
- 단순히 노드의 값을 변경하는 것도 안 되고 노드만 바꾸는 것이 아니라 가리키는 다음 노드도 바꿔주어야 하기 때문이었습니다.
- 문제를 단순화해서 보면, 인접한 두 노드(left, right)의 순서를 바꾸어주는 것과 left 노드가 다음 left를 가리켜야 한다는 것인데요!
- 저는 left 노드가 다음 right 노드를 가리켜야 한다고 생각을 해서 애를 먹었습니다. 예제를 한 번 확인해보겠습니다.
- \[1, 2, 3, 4\] 는 스왑 이후에 \[2, 1, 4, 3\] 이 됩니다. 처음에는 left = 1, right = 2죠?! 그 다음 연산에서는 left = 3, right = 4 가 될 것입니다.
- 처음 두 노드를 스왑한다면 right.next = left가 될 것이므로 2.next = 1, 1.next 는 다음 노드의 right인 4 로 보이기 때문입니다.
- 그러나 이것은 1, 2의 연산과 3, 4의 연산을 하나의 연산으로 보았기 때문에 나타난 오류입니다. 문제를 가장 작은 단위로 쪼갠다면 1, 2 연산과 3, 4의 연산은 분리가 되어야 하죠!
- 따라서 1.next는 다음 노드들이 연산을 어떻게 하는지 알 필요가 없이 그저 left 인 3을 가리켜주면 됩니다. 그 이후 연산에서 left와 right가 스왑되면서 1.next는 자연스럽게 4를 가리키게 될 테니까요!


### 🔍 정답(반복문)

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
    public ListNode swapPairs(ListNode head) {
        ListNode newNode = new ListNode(0);
        newNode.next = head;
        ListNode currentNode = newNode;

        while (currentNode.next != null && currentNode.next.next != null) {
            ListNode left = currentNode.next;
            ListNode right = currentNode.next.next;

            currentNode.next = right;
            left.next = right.next;
            right.next = left;
            currentNode = left;
        }

        return newNode.next;
    }
}
```

- 따라서 left와 right 노드를 정의해주고 다음 노드들을 조정해주면 됩니다.
- 이 때 조정의 순서도 신경써주어야 하는데 이 부분도 생각하기 쉽지 않았습니다 ㅠㅠ


### 🔍 정답(재귀)

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
    public ListNode swapPairs(ListNode head) {
        if (head == null || head.next == null) return head;

        ListNode target = head.next;
        ListNode next = target.next;
        target.next = head;
        head.next = swapPairs(next);

        return target;
    }
}
```

- 다른 분은 재귀를 이용해서 풀이를 하였는데요!
- 기본적으로 left, right를 스왑하고 left.next를 다음 노드의 헤드로 설정하는 것은 반복문 풀이와 동일합니다.