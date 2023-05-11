# 이진 트리

- 자식 노드가 최대 두 개인 노드들로 구성된 트리

## 이진 트리 순회

- 루트 노드를 언제 탐색하느냐에 따라 순회 방법이 나누어진다.
- 루트 노드 탐색(V), 왼쪽 노드 탐색(L), 오른쪽 노드 탐색(R)이라고 한다!

### 전위 순회(Preorder) - VLR
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbBYcIv%2FbtseHYgyQPt%2FAFFhMxYLsbXXpHEQx2wHik%2Fimg.png)


### 중위 순회(Inorder) - LVR
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FIcUlm%2FbtseIFnw23W%2F3kqTeEnkFZa79I49Q3ltH1%2Fimg.png)


### 후위 순회(Postorder) - LRV
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdiWSdz%2FbtseKeXscmB%2FAjkKCGiRnXU8260nlQgiFK%2Fimg.png)


#### 코드 구현

```java
public class Node {
	int data;  // 노드의 값
	Node left;
	Node right;

	public Node(int data) {
		this.data = data;
	}

	/**
	 * 이진 탐색 트리로 만들기
	 * 현재 데이터가 비교값(num)보다 크다면
	 * 비교값을 왼쪽으로 보낸다.
	 * 이 때 왼쪽에 값이 있다면 그 값으로 재귀호출을 해서 더 깊이 탐색을 한다!
	 */
	public void tree(int num) {
		if (num < this.data) {
			if (this.left == null) {
				this.left = new Tree(num);
			} else {
				this.left.tree(num);
			}
		} else {
			if (this.right == null) {
				this.right = new Tree(num);
			} else {
				this.right.tree(num);
			}
		}
	}
}
```

##### 전위 순회

```java
public static void preOrder(Node node) {
	System.out.println(node.data);
	
	if (node.left != null) {
		preOrder(node.left);
	}
	
	if (node.right != null) {
		preOrder(node.right);
	}
}
```

##### 중위 순회

```java
public static void inOrder(Node node) {
	if (node.left != null) {
		inOrder(node.left);
	}
	
	System.out.println(node.data);

	if (node.right != null) {
		inOrder(node.right);
	}
}
```


##### 후위 순회

```java
public static void postOrder(Node node) {
	if (node.left != null) {
		postOrder(node.left);
	}

	if (node.right != null) {
		postOrder(node.right);
	}

	System.out.println(node.data);
}
```


# 이진 탐색 트리

- 이진 트리 중 특정 조건에 따라 정렬이 되어있는 트리
	- 부모 노드의 왼쪽 노드는 부모 노드보다 작다.
	- 부모 노드의 오른쪽 노드는 부모 노드보다 크다.


## 이진 탐색 트리의 탐색

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcye43Q%2FbtseIIkdBAq%2FKL4IoVQ6xGhRfBFq2bOqPk%2Fimg.png)

- 위 트리에서 7을 찾고자 한다면,
- "7"이 Root(8) 보다 큰가 작은가? : 작음 (작다면 왼쪽 크다면 오른쪽 노드로 옮겨간다, 왼쪽 노드(3)로 이동)  
- "7"이 Node(3) 보다 큰가 작은가? : 큼 (오른쪽 노드(6)로 이동)  
- "7"이 Node(6) 보다 큰가? 작은가? : 큼 (오른쪽 노드(7)로 이동)  
- "7"과 Node(7)의 값이 동일하다 : 탐색 종료


---
### 참고 사이트

- [\[유튜브\] Binary Tree의 3가지 순회방법 구현하기](https://www.youtube.com/watch?v=QN1rZYX6QaA)
- [\[자료구조\] 이진트리의 순회 및 java구현](https://go-coding.tistory.com/8)