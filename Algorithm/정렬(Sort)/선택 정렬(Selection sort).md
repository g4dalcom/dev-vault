# 개념

선택 정렬은 **현재 위치에 들어갈 데이터를 찾아서 선택** 하는 정렬 방법이다.

데이터를 비교하면서 찾기 때문에 **비교 정렬** 이며 추가적인 공간을 필요로 하지 않기 때문에 **제자리 정렬** 이다.

또한 정렬을 할 때 처음 순서를 보장하지 않기 때문에 **불안정 정렬** 이기도 하다.

시간 복잡도는 $$O(N^2)$$


# 흐름

1. 주어진 리스트에서 최솟값을 찾는다.
2. 최솟값을 맨 앞자리의 값과 교환한다.
3. 정렬된 부분을 제외한 나머지 값들 중 최솟값을 찾아서 반복한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FzGdeV%2Fbtsh59ZeYkR%2F12lOZZqZW99jsD6Gi7QoC0%2Fimg.png)


# 코드 구현

```java
public class Solution {
	public void Selection_sort(int[] arr) {
		for (int i = 0; i < arr.length; i++) {
			int min_index = i;

			for (int j = i+1; j < arr.length; j++) {
				if (arr[j] < arr[min_index]) {
					min_index = j;
				}
			}
		}
		swap(arr, min_index, i);
	}

	public static void swap(int[] arr, int i, int j) {
		int temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
	}
}
```

