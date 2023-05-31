# 개념

힙 정렬을 이야기 하기 전에 힙 자료구조에 대해 먼저 알아야 한다. 그런데 힙을 알기 위해서는 또 이진트리에 대해서 알고 있어야 한다.

따라서 이진 트리부터 빠르게 훑어보고자 한다!

## 이진 트리

### 이진 트리

이진트리란 모든 노드의 최대 차수를 2로 제한한 트리이다. 따라서 자식 노드는 0~2개를 갖게 된다.


### 완전 이진 트리

완전 이진 트리는 이진트리에서 두 가지 조건이 더 충족되어야 한다.

1. 마지막 레벨을 제외한 모든 노드가 채워져있어야 한다.
2. 모든 노드들은 왼쪽부터 채워져야 한다.


### 포화 이진 트리

포화 이진 트리는 완전 이진 트리의 조건에서 마지막 레벨을 제외한 모든 노드는 두 개의 자식노드를 갖는다. 라는 조건을 추가로 가져야 한다.


## 힙(Heap)

힙은 **최솟값** 또는 **최댓값**을 **빠르게** 찾아내기 위해 **완전이진트리** 형태로 만들어진 자료구조이다.

> 부모 노드는 항상 자식 노드보다 우선순위가 높다.

위 명제를 고려하여 완전이진트리 형태로 채워나가는 자료구조이다.


#### 힙의 종류

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwX4VU%2FbtsiaY5tIdb%2FsGnoxCHBjisAlcoKZbm9tk%2Fimg.png)


이러한 힙 자료구조에서는 불변하는 성질이 있다.

1. 부모 노드 인덱스 = (자식 노드 인덱스 - 1) / 2
2. 왼쪽 자식 인덱스 = 부모 노드 인덱스 X 2 + 1
3. 오른쪽 자식 인덱스 = 부모 노드 인덱스 X 2 + 2


30을 예로 들면 인덱스 1에 위치하고 있고
왼쪽 자식은 1 X 2 + 1 = 3번 인덱스, 오른쪽 자식은 1 X 2 + 2 = 4번 인덱스에 위치하고 있는 것이다.


## 정렬 방법

힙 정렬에서는 **최대 힙** 조건을 만족하도록 완전 이진 트리를 만들어야 한다.

힙은 항상 부모 노드가 자식 노드보다 우선순위가 높은데 자식 노드들, 즉 형제 노드의 우선순위는 고려되지 않는다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb4BMTo%2FbtsicllnymC%2FLwrrg4GUxfbgPGpsHKeI8k%2Fimg.png)

따라서 최소 힙의 경우는 위와 같은 형태도 완전 이진 트리의 조건을 만족한 것이 되어버린다.

그렇기 때문에 최대힙으로 구현해서 **루트 노드가 가장 큰 값**을 갖게 한 후 **루트 노드를 하나씩 삭제하며 뒤에서부터 배치** 하도록 구현을 해야 하는 것이다.


## 구현

#### 과정 1. 최대 힙 만들기(heapify)

```java
public class HeapSort {
	private static void heapify(int[] arr, int parentIdx, int lastIdx) {
		int left = 2 * parentIdx + 1;
		int right = 2 * parentIdx + 2;
		int largest = parentIdx;

		// 비교
		if (left < lastIdx && arr[largest] < arr[left]) {
			largest = left;
		}

		if (right < lastIdx && arr[largest] < arr[right]) {
			largest = right;
		}

		if (parentIdx != largest) {
			int temp = arr[largest];
			arr[largest] = arr[parentIdx];
			arr[parentIdx] = temp;
			heapify(arr, largest, lastIdx);
		}
	}
}
```

부모 노드와 자식 노드를 비교한 후 가장 큰 값을 부모 노드의 인덱스로 위치시킨다.
이 때, 교환된 자식 노드의 서브트리도 힙을 만족해야 하므로 재귀호출을 해서 교환된 자식 노드의 인덱스(largest)를 부모 노드 파라미터로 넘겨준다!


가장 첫 번째로 검사하는 노드는 **가장 마지막 노드의 부모 노드 서브트리**이다.

arr = {3, 7, 5, 4, 2, 8} 일 때,
가장 마지막 노드는 arr.length-1 = 5가 될 것이고
이 노드의 부모 노드는 위에서 힙 자료구조의 성질에 대해 이야기 했듯이 (자식 노드 인덱스 - 1) / 2 = 2가 된다.

따라서 인덱스 2부터 시작해서 해당 인덱스의 서브트리에 대해 힙을 만족시키는 것이다.

```java
public class HeapSort {
	public static void heapsort(int[] arr) {
		// max heap 만들기
		for (int i = arr.length/2 -1; i >= 0; i--) {
			heapify(arr, i, arr.length);
		}
	}
}
```

위와 같이 부모노드(i)를 1씩 줄이면서 heap조건을 만족해나간다.



#### 과정 2. 정렬(오름차순 기준)

위처럼 최대힙을 만들었다면 **루트 노드**가 최댓값이 되어있다.

1. 최댓값이 루트 노드를 맨 뒤로 보냄
2. 뒤에 채운 원소를 제외한 나머지 배열 원소들을 다시 최대힙이 만족하도록 재구성

```java
for (int i = arr.length-1; i > 0; i--) {
	int temp = arr[i];
	arr[i] = arr[0];
	arr[0] = temp;
	heapify(arr, 0, i);
}
```


## 전체 구현 코드

```java
public static void heapsort(int[] arr) {
	// max heap 만들기
	for (int i = arr.length/2 - 1; i >= 0; i--) {
		heapify(arr, i, arr.length);
	}

	// 정렬
	for (int i = arr.length-1; i > 0; i--) {
		int temp = arr[i];
		arr[i] = arr[0];
		arr[0] = temp;
		heapify(arr, 0, i);
	}

	private static void heapify(int[] arr, int parentIdx, int lastIdx) {
		int left = 2 * parentIdx + 1;
		int right = 2 * parentIdx + 2;
		int largest = parentIdx;

		if (left < lastIdx && arr[largest] < arr[left]) {
			largest = left;
		}

		if (right < lastIdx && arr[largest] < arr[right]) {
			largest = right;
		}

		if (parentIdx != largest) {
			int temp = arr[largest];
			arr[largest] = arr[parentIdx];
			arr[parentIdx] = temp;
			heapify(arr, largest, lastIdx);
		}
	}
}
```


여기서 heapify 부분이 재귀호출로 구현이 되어 있는데 이 부분을 반복문으로 구현할 수도 있다.

```java
public static void heapsort(int[] arr) {
	// max heap 만들기
	for (int i = arr.length/2 - 1; i >= 0; i--) {
		heapify(arr, i, arr.length);
	}

	// 정렬
	for (int i = arr.length-1; i > 0; i--) {
		int temp = arr[i];
		arr[i] = arr[0];
		arr[0] = temp;
		heapify(arr, 0, i);
	}

	private static void heapify(int[] arr, int parentIdx, int lastIdx) {
		int left;
		int right;
		int largest;

		while ((parentIdx * 2) + 1 <= lastIdx) {
			left = (parentIdx * 2) + 1;
			right = (parentIdx * 2) + 2;
			largest = parentIdx;

			if (arr[left] > arr[largest]) {
				largest = left;
			}

			if (rigth <= lastIdx && arr[right] > arr[largest]) {
				largest = right;
			}

			if (largest != parentIdx) {
				int temp = arr[largest];
				arr[largest] = arr[parentIdx];
				arr[parentIdx] = temp;
			} 
			else return;
		}
	}
}
```