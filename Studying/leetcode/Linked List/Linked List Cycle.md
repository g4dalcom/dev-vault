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