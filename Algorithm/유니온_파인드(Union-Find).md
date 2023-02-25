# 개념

- 그래프 알고리즘의 하나로, 여러 개의 노드 중 선택한 두 노드가 같은 그래프상에 위치하는지 판별하는 알고리즘이다.
- `합집합 찾기` 로도 불리며, 반대로 서로 연결되어 있찌 않은 노드를 판별할 수도 있기 때문에 `서로소 집합(Disjoint-set)` 이라고도 한다.
- 같은 그래프에 있는 노드를 합치는 `Union` 연산과 노드의 루트를 찾는 `Find` 연산으로 이루어진다.

# 알고리즘 흐름

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FHsSq5%2Fbtr0BDycvgp%2FGIP7KvbmYo72c4ePrsSaDK%2Fimg.png)

- 다섯 개의 노드가 있다고 가정한다.
- 초깃값으로 각 노드들은 자기 자신을 부모로 갖는다.

```java
for (int i = 0; i < n; i++) {
	parent[i] = i;
}
```


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FmylEs%2Fbtr0DVSWu5S%2FkXtubaek6yMSk7ZviCzA7K%2Fimg.png)

- 1과 2, 그리고 3과 4가 같은 그래프에 위치하였다고 하면 Union 연산을 하게 된다.
- 일반적으로 Union시, 더 작은 값쪽으로 합치기 때문에 이 연산의 결과로 parent[2] = 1, parent[4] = 3 이 된다.

```java
Union(1, 2);
Union(3, 4);
```

```java
public static void Union(int a, int b) {
	if (a != b) {
		if (a < b) {
			parent[b] = a;
		} else {
			parent[a] = b;
		}
	}
}
```


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fmfk17%2Fbtr0NNFxduf%2F2K8E5WfvLFrA4SyGWRsPFK%2Fimg.png)

- 그런데 만약 위 그림처럼 연결이 되었다면 1과 3이 같은 그래프상에 위치한다고 바로 알기가 어렵다.
- 이 때 사용하는 것이 Find 연산이다!
- 재귀함수를 이용하여 3의 부모인 2를 먼저 찾고 2의 부모인 1을 파악하여 parent[3] = 1 이라는 값을 만들어주는 것이다.

```java
public static void Union(int a, int b) {
	a = find(a);
	b = find(b);

	if (a != b) {
		if (a < b) {
			parent[b] = a;
		} else {
			parent[a] = b;
		}
	}
}
```

```java
public static int Find(int x) {
	if (x == parent[x]) {
		return x;
	}
	return parent[x] = Find(parent[x]);
}
```
- x = 3 이라고 하면, parent[3] = 2이기 때문에
	- parent[3] = Find(2);
	- x = 2, parent[2] = 1
		- parent[2] = Find(1);
		- 1 == parent[1] 이므로 return 1


![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbkCI4g%2Fbtr0RQopUVi%2F2VsAOSoEgkeEaxrKzKXj1k%2Fimg.png)
- 최종적으로 위와 같은 그림으로 parent가 채워진다.

[관련 문제](/Algorithm/백준(골드5)/집합의 표현(백준1717번)_골드5_union-find(유니온 파인드).md)