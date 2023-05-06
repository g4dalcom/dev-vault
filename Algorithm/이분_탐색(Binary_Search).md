# 이분 탐색, 이진 탐색


![](https://blog.kakaocdn.net/dn/d5jiRY/btrS9pZGDEC/n9uJyExA6AQg6zPAKj5eIK/img.gif)

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

### 소스 코드 - 반복문

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

### 소스코드 - 재귀

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

# lower_bound(하한), upper_bound(상한)
- 이진 탐색이 특정 값을 정확히 찾는 것이라면 상한, 하한 알고리즘은 데이터 내에 중복된 자료가 있을 경우에 유용하게 사용된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FyYe2l%2Fbtsd3dZsBgr%2F05OzqgWDqjI25C9xooJkj1%2Fimg.png)

- lower bound는 찾고자 하는 값 이상의 값이 처음 나타나는 위치
- upper bound는 찾고자 하는 값을 초과한 처음 위치
	- 찾고자 하는 값 : 4
	- 주어진 배열 : 1 2 2 4 4 4 6 7 7 9
	- lower bound는 4 이상이 처음 나타나는 3번 인덱스
	- upper bound는 4 초과하는 값이 처음 나타나는 6번 인덱스

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb3fL7h%2FbtsdZZOEvzv%2FhWKhjmE9k6ywiFKxOx2iRk%2Fimg.png)


### Lower bound Algorithm

```java
private static int lowerBound(int[] arr, int key) {
	int lo = 0; 
	int hi = arr.length; 
 
	// lo가 hi랑 같아질 때 까지 반복
	while (lo < hi) {
 
		int mid = lo + ((hi - lo)/2);
 
		/*
		 *  key 값이 중간 위치의 값보다 작거나 같을 경우		 *  (중복 원소에 대해 왼쪽으로 탐색하도록 상계를 내린다.)
		 */
		if (key <= arr[mid]) {
			hi = mid;
		}
 
		else {
			lo = mid + 1;
		}
 
	}
	return lo;
}
```


### Upper bound Algorithm

```java
private static int upperBound(int[] arr, int key) {
	int lo = 0; 
	int hi = arr.length; 
 
	// lo가 hi랑 같아질 때 까지 반복
	while (lo < hi) {
 
		int mid = lo + ((hi - lo)/2);
 
		// key값이 중간 위치의 값보다 작을 경우
		if (key < arr[mid]) {
			hi = mid;
		}
		// 중복원소의 경우 else에서 처리된다.
		else {
			lo = mid + 1;
		}
	}
	return lo;
}
```

#### 중간값(mid) 계산 로직

- 현재 문제에서는 중간 값을 구할 때 mid = (lo + hi) / 2 를 해도 문제가 없지만, 사실 **값의 범위가 클 경우**에는 **int overflow가 발생할 수 있다.**
	- lo = 1, hi = 2147483647 (= Integer.MAX_VALUE) 이렇다면,
	- (lo + hi) 과정에서 overflow가 발생하여 -2147483648 가 되고, 여기에 2를 나누게 되어 -1073741824 라는 잘못된 값을 반환할 수 있다.

- 이러한 경우 어떻게 해결하냐면, 결국 lo와 hi의 중간 값이라는 것은 lo 로부터 hi까지의 거리를 2로 나눈 값을 더한 값이라는 것이다.
- lo = 3, hi = 7 이라 할 때, 중간 값은 (3 + 7) / 2 = 5 일 것이다.
- 이렇게 위와 같이 표현 할 수 있지만, 사실 생각해보면, 3 ~ 7까지 거리라는 건  결국 3을 기준으로 7이 얼만큼 떨어져 있는가이다.
- 즉, 3으로부터 4만큼 떨어져있는 지점은 7이라는 것이고, 만약 두 지점의 중간 값이라면, **떨어진 거리의 절반**이라는 것이다.
- 그러면 다음과 같이 표현 할 수 있다.
- **중간 지점 = 시작점 + (거리 차) / 2**
- **mid = lo + ((hi - lo) / 2)**


#### hi의 초깃값 설정

- lower bound나 upper bound의 경우 배열 내에 있는 수들보다 찾으려는 수가 더 크다면 배열의 크기를 초과하는 인덱스를 가리키게 될 수가 있다.

```java
int[] arr = {1, 2, 2, 3, 3, 4, 5};
int target = 6;
```

- 위와 같은 경우 주어진 배열의 크기만큼 리턴을 해야하기 때문에 hi = arr.length-1; 이 아니라 **hi = arr.length;** 가 되는 것이다!

---
### 참고 자료

- [참고 사이트 1](https://st-lab.tistory.com/267)
- [참고 사이트 2](https://jackpot53.tistory.com/33)