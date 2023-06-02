# 개념

기수 정렬은 카운팅 정렬처럼 데이터끼리 비교하지 않는 **비-비교정렬**방식이다.
데이터의 초기 순서를 보장하는 **안정 정렬**이며 추가적인 메모리 공간(bucket)을 사용하는 **비-제자리정렬**이다.

**LSD(Least Significant Digit, 가장 작은 자릿수부터 정렬)** 와 **MSD(Most Significant Digit, 가장 큰 자릿수부터 정렬)** 의 두 가지 정렬 방식이 존재한다.

정수, 알파벳 등 아스키 문자로 표현할 수 있는 것들만 정렬이 가능하고 음수와 양수를 비교할 수 없다는 점, 추가적인 메모리 공간(bucket)이 필요하다는 점 등 여러가지 제약 조건과 제한적인 범위 때문에 범용적으로 사용되지는 않는다고 한다.


## 흐름(LSD 기준)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FQ9eD2%2FbtsilJGt37H%2FgyKdKPiELwJkmAn01MUnPk%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbLldi2%2FbtsijLrXd8U%2FLVyWCJTlgivAtLXRSPMD7k%2Fimg.png)

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbpGL4e%2FbtsijOu8P6c%2FWRxjcVLSbaIymvHGjk7swk%2Fimg.png)

1. 큐(Queue)를 이용해 저장공간(bucket) 생성한다.
2. 1의 자리를 기준으로 해당 버킷에 넣는다.
3. 버킷에 저장된 순서대로 꺼내서 재배열한다.
4. 자릿수를 바꿔서 2~3을 반복한다.

이 때, 비교하는 자릿수가 비어있는 경우 (100의 자리를 비교하는데 10의 자리까지만 있는 경우)는 0으로 보고 계산한다.


## 코드 구현

```java
public class Solution {
	// 10진수 계산
	static final int DIGITS = 10;

	public void radixSort(int[] arr) {
		// 과정 1. 카운팅 정렬을 위한 최댓값 찾기
		int m = getMax(arr);

		// 과정 2. 1의 자릿수부터 카운팅 정렬
		int exp = 1;
		while (m / exp > 0) {
			countingSort(arr, exp);
			// 과정 3. 자릿수 늘려가며 반복
			exp *= 10;
		}
	}

	public void countingSort(int[] arr, int exp) {
		// 숫자일 경우 0~9, 알파벳일 경우는 26
		int[] count = new int[10];

		// 자릿수(exp)에 따라 카운팅 배열에 저장
		for (int i : arr) {
			int cur = i / exp;
			count[cur % DIGITS]++;
		}

		// 카운팅 누적
		for (int i = 1; i < count.length; i++) {
			count[i] += count[i-1];
		}

		// 정렬
		int[] result = new int[arr.length];
		for (int i = arr.length-1; i >= 0; i--) {
			int cur = arr[i] / exp;
			result[count[cur % DIGITS]-1] = arr[i];
			count[cur % DIGITS]--;
		}

		for (int i = 0; i < arr.length; i++) {
			arr[i] = result[i];
		}
	}

	public int getMax(int[] arr) {
		int max = arr[0];
		for (int i : arr) {
			if (max < i) max = i;
		}
		return max;
	}
}
```