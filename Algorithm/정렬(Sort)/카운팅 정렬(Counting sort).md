# 개념

카운팅 정렬은 시간복잡도가 O(n)이다. 퀵 정렬이나 힙 정렬 등이 평균적으로 O(nlogn)인 것에 비하면 엄청난 성능이다.

기본적으로 정렬은 데이터끼리 비교하는 경우가 많은데 카운팅 정렬은 인덱스를 가지고 위치를 찾아가는 방식이기 때문에 이러한 속도가 가능하다.

그러나 일반적인 상황에서는 객체를 가지고 정렬을 하는 경우가 많기 때문에 특정 타입에 한정되어 있는 카운팅 정렬은 쓰임이 많지 않기도 하고, 또한 수의 범위가 클 경우 메모리 낭비가 심하다는 단점도 있다.


## 흐름

### 과정 1

주어진 배열(array)이 있을 때, array에 입력될 수 있는 수의 범위를 크기로 갖는 새로운 배열(counting)을 만든다.

그리고 array를 순회하면서 해당 값에 대응하는 counting의 인덱스를 증가시켜준다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIjkjv%2Fbtsg3WNvKSr%2FA7j6JxpNQejlhIzC8SwRX0%2Fimg.png)

카운팅 배열에는 각 값의 개수가 담기게 된다.


### 과정 2

counting 배열의 각 값들을 누적합으로 변환시킨다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbXJ9Bw%2Fbtsg0oEzhtM%2F4l7GgGEgYQZmy8LvZ2xU0k%2Fimg.png)

이러한 결과로 다음 두 배열을 갖게 된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdYd7E5%2Fbtsg2knQuPT%2FVTsPYYioBtnTkQ8qeB3830%2Fimg.png)


### 과정 3

counting 배열의 각 값은 **시작점 - 1** 을 알려준다.

array\[11\] = 1이고
그 값을 인덱스로 갖는 counting\[1\] = 3 이다.
counting 의 값은 시작점 - 1 이므로 3 - 1 = 2를 해주면
처음에 탐색하던 1이라는 값이 2번 인덱스에 위치하게 된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbgQMmm%2Fbtsg25cO6DG%2FKcSde6r3XxkPOxpo2Wg4fK%2Fimg.png)

이렇게 모든 요소를 순회하면 신기하게 정렬된 상태가 된다.


## 구현

```java
public class CountingSort {
	public static void main(String[] args) {
		int[] array = new int[100];  // 원소 개수
		int[] counting = new int[31];  // 수의 범위 : 0 ~ 30
		int[] result = new int[100];  // 정렬될 배열

		// 과정 1
		for (int i = 0; i < array.length; i++) {
			counting[array[i]]++;
		}

		// 과정 2
		for (int i = 1; i < counting.length; i++) {
			counting[i] += counting[i-1];
		}

		// 과정 3
		for (int i = array.length - 1; i >= 0; i--) {
			int value = array[i];
			counting[value]--;
			result[counting[value]] = value;
		}
	}
}
```

---

### 참고 자료

[자바 [JAVA] - 카운팅 정렬 (Counting Sort / 계수 정렬)](https://st-lab.tistory.com/104)