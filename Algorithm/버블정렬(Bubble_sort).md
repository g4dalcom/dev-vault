# 개념

- 선택 정렬과 유사한 알고리즘으로 **서로 인접한 두 원소의 대소를 비교하고 더 큰 원소를 오른쪽으로 옮기며 정렬**한다.


# 정렬 방법

![](https://blog.kakaocdn.net/dn/dEC5xy/btsamWtPejY/X8X8EhGpnugjT4nrnOjBcK/img.gif)

- 원소의 개수만큼 탐색을 한다.
- 현재 원소와 바로 이전 원소의 대소를 비교해서 만약 이전 원소가 더 크다면 현재 원소와 자리를 바꾼다.
- 모든 원소 비교가 끝나면 가장 큰 원소가 제일 오른쪽에 위치하므로 다음 회전에서는 마지막 원소는 제외하고 비교를 하게 된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc7M8nq%2Fbtsah9VkSiY%2FReYJanYV0RF2PlaelgDvQk%2Fimg.png)
- 한 번의 탐색이 끝나면 가장 큰 수가 맨 마지막에 위치하며 정렬이 완료되므로 그 다음 탐색부터는 마지막 인덱스를 제외하고 탐색을 한다.
- 따라서 전체 원소의 개수가 n개라면 총 n-1번 순회하게 된다.

$$(n-1) + (n-2) + ... + 2 + 1 = (n-1) \times \frac{n}{2} = \frac{n(n-1)}{2}$$
- 시간복잡도는 $$O(n^2)$$
- 버블정렬은 구현이 간단하지만 데이터가 많아질 수록 성능이 저하되기 때문에 주로 데이터의 개수가 적을 때 사용하게 된다.

# 코드 구현

### 자바 코드

```java
void BubbleSort(int[] arr) {
	int temp = 0;
	for (int i = 0; i < arr.length; i++) {
		for (int j = 1; j < arr.length-1; j++) {
			if (arr[j-1] > arr[j]) {
				temp = arr[j-1];
				arr[j-1] = arr[j];
				arr[j] = temp;
			}
		}
	}
	return arr;
}
```

### 자바스크립트(성능 개선)

```javascript
const bubbleSort = (arr) => {
	for (let i = 0; i < arr.length; i++) {
		let cnt = 0;
		
		for (let j = 1; j < arr.length; j++) {
			if (arr[j-1] > arr[j]) {
				[arr[j-1], arr[j]] = [arr[j], arr[j-1]];
				cnt++
			}
		}
		if (cnt === 0) break;
	}

	return arr;
}
```


---

### 참고 자료
- [Bubble Sort : 거품 정렬](https://bowbowbow.tistory.com/10)
- [Tech Interview, 거품 정렬(Bubble Sort)](https://gyoogle.dev/blog/algorithm/Bubble%20Sort.html)