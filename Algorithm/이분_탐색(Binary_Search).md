![](https://blog.penjee.com/wp-content/uploads/2015/04/binary-and-linear-search-animations.gif)

- 이분 탐색(이진 탐색) 알고리즘은 `정렬되어 있는 리스트`에서 탐색 범위를 절반씩 좁혀가며 데이터를 탐색하는 방법이다.
- 변수 3개(start, mid, end)를 사용해서 탐색을 한다.
- 찾으려는 데이터와 중간점 위치에 있는 데이터를 반복적으로 비교해서 원하는 데이터를 찾는다.
- 예시
	- 1 2 3 4 5 6 7 8 9 10 에서 7 이라는 숫자를 찾는다고 할 때,
	- 처음에는 start = 0, end = max(10) 이다.
	- 1~10 중간인 5(mid)와 7을 비교한다. 5 < 8 이므로 5 보다 작은 이전 인덱스들은 탐색할 필요가 없어진다.
		- start = mid + 1
	- 6 7 8 9 10 중 중간인 8(mid)과 7을 비교한다. 8 < 7 이므로 8보다 큰 인덱스들은 탐색하지 않는다.
		- end = mid - 1
	- 6 7 8 중 중간인 7(mid) = 7(찾고자 하는 수) 이므로 탐색을 종료한다.

- 소스 코드 - 반복문
```java
public static int binarySearch(int arr[], int target) {
  int mid;
  int start = 0;
  int end = arr.length - 1;

  // 배열의 크기가 1이 될 때까지 반복
  while (start <= end) {
    mid = (start + end) / 2;

    // 원하는 값을 찾았다면 mid 반환
    if (arr[mid] == target) {
      return mid;
    }

    if (target < arr[mid]) {
      end = mid - 1;
    } else {
      start = mid + 1;
    }
  }

  // 값을 구할 수 없는 경우
  return -1;
}
```

- 소스코드 - 재귀
```java
public static int binarySearch(int[] arr, int target, int start, int end) {

  // 값을 구할 수 없는 경우
  if (start > end) {
    return -1;
  }

  int mid = (start + end) / 2;
  
  // 원하는 값을 찾았다면 mid 반환
  if (target == arr[mid]) {
    return mid;
  }

  if (target > arr[mid]) {
    return binarySearch(arr, target, mid + 1, end);
  }
  return binarySearch(arr, target, start, mid - 1);
}
```

#### lower_bound(하한), upper_bound(상한)
- lower bound는 찾고자 하는 값 이상의 값이 처음 나타나는 위치
- upper bound는 찾고자 하는 값을 초과한 처음 위치
	- 찾고자 하는 값 : 4
	- 주어진 배열 : 1 2 2 4 4 4 6 7 7 9
	- lower bound는 4 이상이 처음 나타나는 3번 인덱스
	- upper bound는 4 초과하는 값이 처음 나타나는 6번 인덱스
