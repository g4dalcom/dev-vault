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


### 반복적 순회

- 반복을 이용해서 순회를 할수 있다.  
- 스택(Stack)을 사용해 자식 노드들을 저장하고 꺼내면서 순회하면 중위 순회화 결과가 동일하다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbwVXPS%2FbtseHomo9gY%2FyYtvAgxwJVnzfZUNoQAiqK%2Fimg.png)

#### 흐름

- 이진트리의 왼쪽 노드들을 null 노드에 접근할때까지 스택에 추가
- null 노드에 도달하면 스택에서 하나씩 pop()을 하면서 pop()으로 삭제된 노드를 출력
- pop() 된 노드의 오른쪽 노드로 이동

#### 코드

```java
public void iterativeOrder(Node root) {
        Stack<Node> stack = new Stack<>();

        //이 조건이 중요하다 root가 null인데 인데  스택에 값이 들어가 있는 경우가 V(루트노드)를 읽는 순간이다.
        while(root !=null || !stack.isEmpty()) {    

            while(root !=null) {
                stack.push(root);
                root = root.getLeftNode();
            }

            root = stack.pop();
            System.out.print(root.getData() + " "); 
            root = root.getRightNode();
        }
    }
```


### 레벨 순회

- 각 노드를 레벨 순으로 검사하는 순회 방법이다. 루트의 레벨이 1이고 아래로 내려갈수록 레벨의 숫자가 증가한다. 
- 같은 레벨에서는 무조건 좌에서 우로 읽는다. 큐(Queue)를 사용한다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FNgg2Y%2FbtseG6M7QMW%2FADnyD8TYWm313iuTw4yjB1%2Fimg.png)

#### 코드

```java
public void levelOrder(Node root) {
        Queue<Node> queue = new LinkedList<>();
        queue.offer(root);

        while(!queue.isEmpty()) {
            Node node = queue.poll(); 
            System.out.print(node.getData() + " ");

            // 노드를 출력할때마다 출력되는 노드의 자식 노드들을 바로 큐에 넣게 됨 
            if(node.getLeftNode() != null) queue.offer(node.getLeftNode());
            if(node.getRightNode() != null) queue.offer(node.getRightNode());
        }
    }
```


---
### 참고 사이트

- [\[유튜브\] Binary Tree의 3가지 순회방법 구현하기](https://www.youtube.com/watch?v=QN1rZYX6QaA)
- [\[자료구조\] 이진트리의 순회 및 java구현](https://go-coding.tistory.com/8)