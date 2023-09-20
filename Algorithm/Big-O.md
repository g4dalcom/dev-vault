- 알고리즘의 성능을 수학적으로 표기해주는 표기법
- 알고리즘의 시간과 공간 복잡도를 표현할 수 있다.
- 알고리즘의 실제 러닝타임이라기보다 데이터나 사용자의 증가율의 따른 알고리즘의 성능을 예측하는 것이 목표이기 때문에 상수와 같은 숫자는 모두 1이 된다.


 O(1) 오원
- 입력 데이터의 크기에 상관없이 언제나 일정한 시간이 걸리는 알고리즘

```java
public boolean bigO(int[] n) {
	return n[0] == 0 ? true : false;
}
```

- 매개변수로 받는 n 배열의 크기에 상관없이 항상 같은 시간이 걸린다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbHcDfH%2FbtsuAtk2kFl%2FOU4y8tHB360QzSL78TpR7K%2Fimg.png)

- 그래프로 나타내면 위와 같다. 데이터가 증가해도 성능에 변화가 없다.

O(n) 오엔

- 입력 데이터의 크기에 비례해서 처리 시간이 걸리는 알고리즘

```java
public int bigO(int[] n) {
	int sum = 0;
	for (int i = 0; i < n.length; i++) {
		sum += n[i];
	}
	return sum;
}
```

- n이 늘어날 때마다 처리가 늘어나게 된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F2Zsrd%2FbtsuGhExTaf%2FO0K70cCnItMukfgB3ZZNO1%2Fimg.png)


O(n^2) 오엔스퀘어

```java
public int bigO(int[] n) {
	int sum = 0;
	for (int i = 0; i < n.length; i++) {
		for (int j = 0; j < n.length; j++) {
			sum += (n[i] + j);
		}
	}
	return sum;
}
```

- 2중 for문일 때,

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbFXADT%2FbtsuIEMTDPr%2F1TjRjVWHszE0caJ33Rx2dk%2Fimg.png)

- n 개의 데이터를 받으면 첫 번째 루프에서 n번 돌면서 각각의 엘리먼트에서 n번씩 다시 돌게 되므로 가로 세로의 면적을 갖게 된다.
- n이 하나 늘어나면 가로, 세로의 n만큼씩 늘어나는 구조이므로 데이터가 커질 수록 처리시간의 부담이 점점 커지게 된다.

O(nm)

```java
public int bigO(int[] n, int[] m) {
	int sum = 0;
	for (int i = 0; i < n.length; i++) {
		for (int j = 0; j < m.length; j++) {
			sum += n[i] + m[j];
		}
	}
	return sum;
}
```

- 두 개의 매개변수를 받아서 각각 루프를 도는 구조

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbdX5ya%2FbtsugiEP0ps%2FoXhVKKgpkGgPFfNE03Ovkk%2Fimg.png)

- n스퀘어와 유사하지만 각 배열의 크기가 다를 수 있기 때문에 구별을 해주어야 한다.
- n스퀘어와 마찬가지의 그래프 모양을 갖는다.
