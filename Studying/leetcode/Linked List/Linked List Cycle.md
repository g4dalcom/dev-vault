### Hashset

```java
public boolean hasCycle(ListNode head) {
		if (head == null || head.next == null)
			return false;
		HashSet<ListNode> nodeSet = new HashSet<>();
		while(head != null) {
			if (nodeSet.contains(head))
				return true;
			nodeSet.add(head);
			head = head.next;
		}
		return false;
	}
```


### Two Pointer

```java
public boolean hasCycleNoExtraSpace(ListNode head) {
		if (head == null || head.next == null) 
			return false;
		ListNode p = head;
		ListNode q = head;
		while(q != null && q.next != null) {
			p = p.next;
			q = q.next.next;
			if (p == q)
				return true;
		}
		return false;
	}
```


### Floyd Cycle

```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode walker = head;
        ListNode runner = head;
        
        while (runner != null && runner.next != null) {
            walker = walker.next;
            runner = runner.next.next;
            
            if (walker == runner) break;
        }
        
        if (runner == null || runner.next == null) return null;
        
        walker = head;
        while (walker != runner) {
            walker = walker.next;
            runner = runner.next;
        }
        
        return walker;
    }
}
```
- [참고](https://mhwan.tistory.com/67)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Ft4Fiv%2FbtsfdXaQwT9%2Fs8968wsJYf0jJwqRiyI4rk%2Fimg.png)
- 사이클이 존재한다면 처음 시작점에서 출발해서 한 칸씩 움직이는 walker와 두 칸씩 움직이는 runner는 한 지점에서 만나게 된다(세모)
- walker의 이동 거리 = p + q
- runner의 이동 거리는 2p + 2q
- 실제 runner의 이동 = p + q + r + q
- 따라서 2p + 2q = p + 2q + r로 표현할 수 있고 식을 정리하면 p = r이 된다.
- 따라서 둘이 만난 지점에서 walker를 다시 시작점으로 되돌려 보내고 서로 한 칸씩 이동한다면, p = r이라는 식에 의해 사이클의 시작점에서 다시 만나게 된다.