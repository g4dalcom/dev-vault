# 개념

- 하나의 리스트를 피벗(Pivot)을 기준으로 두 개의 부분 리스트로 나누어서 하나는 피벗보다 작은 값들의 부분 리스트, 다른 하나는 피벗보다 큰 값들의 부분 리스트로 구분한 뒤 각 부분 리스트에 대해 다시 위 처럼 재귀적으로 수행하여 정렬하는 방식이다.
- 분할정복 알고리즘을 기반으로 하며 데이터를 비교하며 정렬하는 `비교정렬` 이자 데이터의 추가적인 공간을 필요로 하지 않는 `제자리정렬` 이며 두 개의 부분 리스트를 만들 때 서로 떨어진 원소끼리 교환이 일어나기 때문에 `불안정 정렬` 이다.


# 정렬 방법

- 왼쪽/오른쪽 피벗 선택 방식과 중간 피벗 선택 방식이 있다.

### 과정

- 피벗을 하나 선택한다.
- 피벗을 기준으로 왼쪽에서부터는 피벗보다 큰 값을 찾고, 오른쪽에서부터는 피벗보다 작은 값을 찾는다.
- 양 방향에서 찾은 두 원소를 교환한다.
- 왼쪽에서 탐색하는 위치와 오른쪽에서 탐색하는 위치가 엇갈리지 않을 때 까지 2번으로 돌아가 위 과정을 반복한다.
- 엇갈린 기점을 기준으로 두 개의 부분리스트로 나누어 처음으로 돌아가 해당 부분리스트의 길이가 1이 아닐 때 까지 과정을 반복한다. (Divide : 분할)
- 인접한 부분리스트끼리 합친다. (Conqure : 정복)

##### 왼쪽 피벗 선택 방식

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4C1AU%2FbtrYsA22x9M%2FulQ4e1boNjLpvtjgEmbQFK%2Fimg.png)
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbKuVeS%2FbtrYquptgFQ%2FPHpZo4OrKtVBmBasYAXkY0%2Fimg.png)
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FboNMuG%2FbtrYmyTs9hQ%2F7kgOMdMkRiKA4qGgENia0K%2Fimg.png)

##### 중간 피벗 정렬 방식

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FMW5T0%2FbtrYqToHiQK%2FJkalPhtkjEAhHOdqz5N1F0%2Fimg.png)
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F38Cme%2FbtrYq5bz0Uh%2FHNYXtwpKx7DjOQVRkMGjPk%2Fimg.png)


# 코드 구현

### 왼쪽 피벗 정렬 방식

```java
private static void l_pivot_sort(int[] a, int lo, int hi) {

		if(lo >= hi) {
			return;
		}
		
		int pivot = partition(a, lo, hi);	
		
		l_pivot_sort(a, lo, pivot - 1);
		l_pivot_sort(a, pivot + 1, hi);
	}
	
	
	/**
	 * pivot을 기준으로 파티션을 나누기 위한 약한 정렬 메소드
	 * 
	 * @param a		정렬 할 배열 
	 * @param left	현재 배열의 가장 왼쪽 부분
	 * @param right	현재 배열의 가장 오른쪽 부분
	 * @return		최종적으로 위치한 피벗의 위치(lo)를 반환
	 */
	private static int partition(int[] a, int left, int right) {
		
		int lo = left;
		int hi = right;
		int pivot = a[left];		// 부분리스트의 왼쪽 요소를 피벗으로 설정
		
		// lo가 hi보다 작을 때 까지만 반복한다.
		while(lo < hi) {
			
			/*
			 * hi의 요소가 pivot보다 작거나 같은 원소를
			 * 찾을 때 까지 hi를 감소시킨다.
			 */
			while(a[hi] > pivot && lo < hi) {
				hi--;
			}
			
			/*
			 * hi가 lo보다 크면서, lo의 요소가 pivot보다 큰 원소를
			 * 찾을 때 까지 lo를 증가시킨다.
			 */
			while(a[lo] <= pivot && lo < hi) {
				lo++;
			}
			
			// 교환 될 두 요소를 찾았으면 두 요소를 바꾼다.
			swap(a, lo, hi);
		}
		
		
		/*
		 *  마지막으로 맨 처음 pivot으로 설정했던 위치(a[left])의 원소와 
		 *  lo가 가리키는 원소를 바꾼다.
		 */
		swap(a, left, lo);
		
		// 두 요소가 교환되었다면 피벗이었던 요소는 lo에 위치하므로 lo를 반환한다.
		return lo;
	}
 
	private static void swap(int[] a, int i, int j) {
		int temp = a[i];
		a[i] = a[j];
		a[j] = temp;
	}
```


### 중간 피벗 정렬 방식

```java
private static void m_pivot_sort(int[] a, int lo, int hi) {
		
		/*
		 *  lo가 hi보다 크거나 같다면 정렬 할 원소가 
		 *  1개 이하이므로 정렬하지 않고 return한다.
		 */
		if(lo >= hi) {
			return;
		}
		
		int pivot = partition(a, lo, hi);	
		
		m_pivot_sort(a, lo, pivot);
		m_pivot_sort(a, pivot + 1, hi);
	}
	
	
	/**
	 * pivot을 기준으로 파티션을 나누기 위한 약한 정렬 메소드
	 * 
	 * @param a		정렬 할 배열 
	 * @param left	현재 배열의 가장 왼쪽 부분
	 * @param right	현재 배열의 가장 오른쪽 부분
	 * @return		최종적으로 위치한 피벗의 위치(hi)를 반환
	 */
	private static int partition(int[] a, int left, int right) {
		
		// lo와 hi는 각각 배열의 끝에서 1 벗어난 위치부터 시작한다.
		int lo = left - 1;
		int hi = right + 1;
		int pivot = a[(left + right) / 2];		// 부분리스트의 중간 요소를 피벗으로 설정
		
 
		while(true) {
			
			/*
			 * 1 증가시키고 난 뒤의 lo 위치의 요소가 pivot보다 큰 요소를
			 * 찾을 때까지 반복한다.
			 */
			do { 
				lo++; 
			} while(a[lo] < pivot);
 
			
			/*
			 * 1 감소시키고 난 뒤의 hi 위치가 lo보다 크거나 같은 위치이면서
			 * hi위치의 요소가 pivot보다 작은 요소를 찾을 때까지 반복한다.
			 */
			do {
				hi--;
			} while(a[hi] > pivot && lo <= hi);
			
			
			/*
			 * 만약 hi가 lo보다 크지 않다면 swap하지 않고 hi를 리턴한다.
			 */
			if(lo >= hi) {
				return hi;
			}
			
			
			// 교환 될 두 요소를 찾았으면 두 요소를 바꾼다.
			swap(a, lo, hi);
		}
		
	}
	
	
	private static void swap(int[] a, int i, int j) {
		int temp = a[i];
		a[i] = a[j];
		a[j] = temp;
	}
```

---
### 관련 자료

- [참고 사이트](https://st-lab.tistory.com/250)