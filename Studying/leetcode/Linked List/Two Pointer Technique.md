## 📝 Intersection of Two Linked Lists(두 연결 목록의 교집합)

- 두 단일 연결 리스트 headA와 headB의 헤드가 주어지면 두 리스트가 교차하는 노드를 반환합니다. 두 개의 연결된 목록에 교차가 전혀 없으면 null을 반환합니다.
- 예를 들어, 다음 두 연결 목록은 노드 c1에서 교차하기 시작합니다.
![](https://assets.leetcode.com/uploads/2021/03/05/160_statement.png)
- 테스트 케이스는 연결된 전체 구조의 어디에도 사이클이 없도록 생성됩니다. 연결된 목록은 함수가 반환된 후에도 원래 구조를 유지해야 합니다.
	- intersectVal - 교차가 발생하는 노드의 값입니다. 
	- 교차 노드가 없으면 0입니다. 
	- listA - 첫 번째 연결 목록입니다. 
	- listB - 두 번째 연결 목록입니다.
	- skipA - 교차된 노드에 도달하기 위해 listA(헤드에서 시작)에서 앞으로 건너뛸 노드 수입니다. 
	- skipB - 교차 노드에 도달하기 위해 listB(헤드에서 시작)에서 앞으로 건너뛸 노드 수입니다
- 그런 다음 심사위원은 이러한 입력을 기반으로 연결된 구조를 생성하고 두 개의 헤드(headA 및 headB)를 프로그램에 전달합니다. 교차된 노드를 올바르게 반환하면 솔루션이 수락됩니다

### Example 1 :

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_1_1.png)

```text
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Intersected at '8'
Explanation: 교차 노드의 값은 8입니다(두 목록이 교차하는 경우 0이 아니어야 함). A의 머리부터 [4,1,8,4,5]로 읽힙니다. B의 머리부터 [5,6,1,8,4,5]로 읽힙니다.
A에서 교차된 노드 앞에 2개의 노드가 있습니다. B에서 교차된 노드 앞에 3개의 노드가 있습니다.
- A와 B에서 값이 1인 노드(A에서 2번째 노드와 B에서 3번째 노드)는 서로 다른 노드 참조이므로 교차된 노드의 값은 1이 아닙니다. 즉, A와 B에서 값이 8인 노드(A의 세 번째 노드와 B의 네 번째 노드)는 메모리의 동일한 위치를 가리키는 반면 메모리에서 두 개의 다른 위치를 가리킵니다.
```

### Example 2 :

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_2.png)

```text
Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Intersected at '2'
Explanation: 교차 노드의 값은 2입니다(두 목록이 교차하는 경우 0이 아니어야 함). A의 머리부터 [1,9,1,2,4]로 읽힙니다. B의 머리부터 [3,2,4]로 읽힙니다. A에서 교차된 노드 앞에 3개의 노드가 있습니다. B에서 교차된 노드 앞에 1개의 노드가 있습니다.
```

### Example 3 :

![](https://assets.leetcode.com/uploads/2021/03/05/160_example_3.png)

```text
Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
Output: No intersection
Explanation: A의 머리부터 [2,6,4]로 읽힙니다. B의 머리부터 [1,5]로 읽습니다. 두 목록이 교차하지 않으므로 intersectVal은 0이어야 하고 skipA와 skipB는 임의의 값이 될 수 있습니다. 두 목록이 교차하지 않으므로 null을 반환합니다.
```


### Constraints :

- The number of nodes of `listA` is in the `m`.
- The number of nodes of `listB` is in the `n`.
- `1 <= m, n <= 3 * 104`
- `1 <= Node.val <= 105`
- `0 <= skipA < m`
- `0 <= skipB < n`
- `intersectVal` is `0` if `listA` and `listB` do not intersect.
- `intersectVal == listA[skipA] == listB[skipB]` if `listA` and `listB` intersect.

---

### 🔍 정답

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode p1 = headA;
        ListNode p2 = headB;
        
        while (p1 != p2) {
            p1 = p1 == null ? headB : p1.next;
            p2 = p2 == null ? headA : p2.next;
        }
        
        return p1;
    }
}
```
- 이 풀이는 처음에 보고 어떻게 가능한 거지? 라는 생각을 했다. 이해하고 보니 역시 simple is best라는 생각이 들었다.
- 두 리스트는 길이가 같다면 당연히 요소를 하나씩 옮기다 보면 같은 요소를 찾거나 동시에 null에 도달할 것이다.
- 길이가 다르다면 길이를 맞춰주면 되는데 리스트 A의 길이 + 리스트 B의 길이 = 리스트 B의 길이 + 리스트 A의 길이라는 것을 이용하면 된다.
- 따라서 더 짧은 길이의 리스트가 먼저 null에 도달할 것이므로 null에 도달했을 때 반대쪽 head로 이동하게 해서 순환하도록 하면 둘의 길이는 맞춰지고 교차점에서 동기화된다!

### ⭐ 다른 정답

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA,ListNode headB) {
        int Alength = getLength(headA);
        int Blength = getLength(headB);
        
        while (Alength < Blength) {
            headB = headB.next;
            Blength--;
        }
        
        while (Alength > Blength) {
            headA = headA.next;
            Alength--;
        }
        
        while (headA != headB) {
            headA = headA.next;
            headB = headB.next;
        }
        
        return headA;
    }
    
    public int getLength(ListNode x) {
        int cnt = 0;

        while (x != null) {
            x = x.next;
            cnt++;    
        }

        return cnt;
    }
}
```
- 두 리스트의 길이를 맞춘 뒤 하나씩 이동시키며 일치하는  요소를 찾아서 반환하는 방법! 이해하기 쉽고 직관적인 풀이인 것 같다!

## Summary

### Tips

1. 다음 필드를 호출하기 전에 항상 노드가 null인지 검사해야 한다.
	 - null 노드의 다음 노드를 가져오면 null 포인터 오류가 발생한다. 예를 들어 fast = fast.next.next를 실행하기 전에 fast와 fast.next가 null이 아님을 모두 검사해야함!
2. 루프의 종료 조건을 신중하게 검토해야 한다.
	- 종료 조건이 무한 루프에 빠지지 않도록 고려해야 한다.

### 복잡도

#### 공간 복잡도

- 다른 추가 공간 없이 포인터만 사용하면 공간 복잡도는 O(1)이다.

#### 시간 복잡도
- 매 턴, 두 단계 이동하는 fast와 한 단계 이동하는 slow라는 두 개의 포인터를 사용한다고 가정하고,
- 사이클이 없는 경우, fast는 리스트 끝에 도달하는 데 N/2이 된다. (N = 리스트 길이)
- 사이클이 있는 경우, fast == slow 라는 조건을 만족해야 하므로 M번의 턴이 필요하다(M = 리스트의 사이클의 길이)
- 사이클의 길이는 리스트보다 클 수 없으므로 M <= N이 되고 따라서 최대 O(N)이 된다고 볼 수 있다.