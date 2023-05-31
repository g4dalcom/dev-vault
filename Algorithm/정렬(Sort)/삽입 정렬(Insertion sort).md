# 개념

삽입 정렬은 **현재 비교하고자 하는 target과 그 이전 원소들을 비교하며 자리를 교환**하는 정렬 방법이다.

데이터를 비교하면서 정렬하므로 **비교 정렬**이고 추가적인 공간을 필요로 하지 않기 때문에 **제자리 정렬**이다. 또한 기존의 순서를 유지하는 **안정 정렬**이다.

거의 정렬된 경우 매우 효율적으로, O(N)의 시간복잡도를 갖지만 역순에 가까울 수록 매우 비효율적으로 바뀌어서 최악의 경우는 O(N^2)의 시간복잡도를 갖는다.

거품 정렬이나 선택 정렬과 같은 시간복잡도이지만 데이터 상태에 따라 편차가 있으므로 평균적으로는 좀 더 성능이 좋은 편이다.

# 흐름

1. 현재 타겟과 이전 원소들을 비교한다.(첫 번째 타겟은 두 번째 원소부터 시작)
2. 타겟보다 더 큰 요소가 있다면 위치를 교환한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmnVOx%2Fbtsh2Gximaq%2FO2m6XBf4dtTZUeY6khOayK%2Fimg.png)


# 코드 구현

```java
void Insertion_sort(int[] arr) {
	for (int i = 1; i < arr.length; i++) {
		int target = arr[i];
		int j = i-1;

		while (j >= 0 && target < arr[j]) {
			arr[j+1] = arr[j];
			j--;
		}

		arr[j+1] = target;
	}
}
```


```java
void Insertion_sort(int[] arr) {
	for (int i = 1; i < arr.length; i++) {
		int target = i;

		while (target > 0 && arr[target-1] > arr[target]) {
			int temp = arr[target];
			arr[target] = arr[target-1];
			arr[target-1] = temp;
			target -= 1;
		}
	}
}
```


### 단일 연결 리스트에서 삽입 정렬 구현

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

void Insertion_sort_list(ListNode head) {
	ListNode dummy = new ListNode(0);
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
```