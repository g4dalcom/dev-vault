# [문제링크](https://school.programmers.co.kr/learn/courses/30/lessons/42892)

## 📝 문제

전무로 승진한 라이언은 기분이 너무 좋아 프렌즈를 이끌고 특별 휴가를 가기로 했다.  
내친김에 여행 계획까지 구상하던 라이언은 재미있는 게임을 생각해냈고 역시 전무로 승진할만한 인재라고 스스로에게 감탄했다.

라이언이 구상한(그리고 아마도 라이언만 즐거울만한) 게임은, 카카오 프렌즈를 두 팀으로 나누고, 각 팀이 같은 곳을 다른 순서로 방문하도록 해서 먼저 순회를 마친 팀이 승리하는 것이다.

그냥 지도를 주고 게임을 시작하면 재미가 덜해지므로, 라이언은 방문할 곳의 2차원 좌표 값을 구하고 각 장소를 이진트리의 노드가 되도록 구성한 후, 순회 방법을 힌트로 주어 각 팀이 스스로 경로를 찾도록 할 계획이다.

라이언은 아래와 같은 특별한 규칙으로 트리 노드들을 구성한다.

- 트리를 구성하는 모든 노드의 x, y 좌표 값은 정수이다.
- 모든 노드는 서로 다른 x값을 가진다.
- 같은 레벨(level)에 있는 노드는 같은 y 좌표를 가진다.
- 자식 노드의 y 값은 항상 부모 노드보다 작다.
- 임의의 노드 V의 왼쪽 서브 트리(left subtree)에 있는 모든 노드의 x값은 V의 x값보다 작다.
- 임의의 노드 V의 오른쪽 서브 트리(right subtree)에 있는 모든 노드의 x값은 V의 x값보다 크다.

아래 예시를 확인해보자.

라이언의 규칙에 맞게 이진트리의 노드만 좌표 평면에 그리면 다음과 같다. (이진트리의 각 노드에는 1부터 N까지 순서대로 번호가 붙어있다.)

![tree_3.png](https://grepp-programmers.s3.amazonaws.com/files/production/dbb58728bd/a5371669-54d4-42a1-9e5e-7466f2d7b683.jpg)

이제, 노드를 잇는 간선(edge)을 모두 그리면 아래와 같은 모양이 된다.

![tree_4.png](https://grepp-programmers.s3.amazonaws.com/files/production/6bd8f6496a/50e1df20-5cb7-4846-86d6-2a2f1e70c5da.jpg)

위 이진트리에서 전위 순회(preorder), 후위 순회(postorder)를 한 결과는 다음과 같고, 이것은 각 팀이 방문해야 할 순서를 의미한다.

- 전위 순회 : 7, 4, 6, 9, 1, 8, 5, 2, 3
- 후위 순회 : 9, 6, 5, 8, 1, 4, 3, 2, 7

다행히 두 팀 모두 머리를 모아 분석한 끝에 라이언의 의도를 간신히 알아차렸다.  
  
그러나 여전히 문제는 남아있다. 노드의 수가 예시처럼 적다면 쉽게 해결할 수 있겠지만, 예상대로 라이언은 그렇게 할 생각이 전혀 없었다.

이제 당신이 나설 때가 되었다.

곤경에 빠진 카카오 프렌즈를 위해 이진트리를 구성하는 노드들의 좌표가 담긴 배열 nodeinfo가 매개변수로 주어질 때,  
노드들로 구성된 이진트리를 전위 순회, 후위 순회한 결과를 2차원 배열에 순서대로 담아 return 하도록 solution 함수를 완성하자.

##### 제한사항

- nodeinfo는 이진트리를 구성하는 각 노드의 좌표가 1번 노드부터 순서대로 들어있는 2차원 배열이다.
    - nodeinfo의 길이는 `1` 이상 `10,000` 이하이다.
    - nodeinfo[i] 는 i + 1번 노드의 좌표이며, [x축 좌표, y축 좌표] 순으로 들어있다.
    - 모든 노드의 좌표 값은 `0` 이상 `100,000` 이하인 정수이다.
    - 트리의 깊이가 `1,000` 이하인 경우만 입력으로 주어진다.
    - 모든 노드의 좌표는 문제에 주어진 규칙을 따르며, 잘못된 노드 위치가 주어지는 경우는 없다.

---

##### 입출력 예

|nodeinfo|result|
|---|---|
|[[5,3],[11,5],[13,3],[3,5],[6,1],[1,3],[8,6],[7,2],[2,2]]|[[7,4,6,9,1,8,5,2,3],[9,6,5,8,1,4,3,2,7]]|

---

### 💡 풀이

- 이진트리를 만들고 전위순회, 후위순회를 하는 문제였습니다.
- 이진트리라는 것은 이진 탐색을 할 수 있도록 트리를 구성하는 것으로, 모든 부모 노드는 왼쪽 자식 노드보다 크고 오른쪽 자식 노드보다 작게 구성이 됩니다.
- 이렇게 되면 매 탐색마다 절반씩 탐색 범위가 줄어들어서 이진 탐색을 하는 효과를 얻을 수 있습니다.
- 어쨌든... 순서는 아래와 같습니다.
	- 루트 노드를 구한다.
	- 루트 노드를 시작으로 트리를 구성한다.
	- 순회를 한다.
- 주어진 입력값 \[x, y\] 중 y는 높이를 나타내고 높이가 클 수록 루트 노드에 가까워집니다. 따라서 y값이 가장 큰 수가 루트 노드가 되고 우리는 이 루트 노드부터 뽑아내서 트리를 구성해야 하기 때문에 우선순위큐를 이용하였습니다.

```java
PriorityQueue<Node> pq = new PriorityQueue<>((o1, o2) -> o2.y - o1.y);
	for (int i = 0; i < nodeinfo.length; i++) {
		pq.add(new Node(nodeinfo[i][0], nodeinfo[i][1]));
	}
```

- 또한 우리가 출력해야 하는 값은 입력으로 주어진 순서인 정수값입니다. y를 기준으로 내림차순으로 정렬하되 처음 입력할 때의 순서를 기억하고 있어야 하는 것입니다.

```java
public static class Node {
	static int ORDER = 1;
	int number;
	int x, y;
	
	public Node(int x, int y) {
		this.number = ORDER++;
		this.x = x;
		this.y = y;
	}
}
```

- 그 부분은 Node 클래스 안에서 구현하였습니다. 전역 변수 ORDER를 두고 Node가 만들어질 때마다 1씩 증가하도록 한 것이죠!
- 다음은 트리 부분입니다. 매번 트리의 value를 비교해야 하므로 node.x 값인 value 필드와, 입력 당시의 순서인 number 필드, 그리고 각각 왼쪽 노드와 오른쪽 노드를 갖습니다. 만약 리프 노드라면 left와 right는 null이 되겠죠!

```java
public class Tree {
	int number;
	int value;
	Tree left, right;
	
	public Tree(int number, int value) {
		this.number = number;
		this.value = value;
	}
	
	public void setNode(Node node) {
		if (node.x < this.value) {
			if (this.left == null) {
				this.left = new Tree(node.number, node.x);
			} else {
				this.left.setNode(node);
			}
		} else {
			if (this.right == null) {
				this.right = new Tree(node.number, node.x);
			} else {
				this.right.setNode(node);
			}
		}
	}
}
```

- 트리 클래스를 구성하였다면 루트 노드부터 넣어준 뒤, setNode 작업을 해주면 됩니다.

```java
Node root = pq.poll();
Tree tree = new Tree(root.number, root.x);

while (!pq.isEmpty()) {
	Node node = pq.poll();
	tree.setNode(node);
}
```

- 순회는 루트 노드를 언제 방문하느냐로 기억을 하면 좋습니다. 전위 순회는 루트 노드를 가장 먼저 방문하고 왼쪽 노드 끝까지 간 후 오른쪽 노드를 타고 올라오는 방식이고
- 후위 순회는 가장 왼쪽 리프 노드부터 방문한 후 마지막에 루트 노드를 방문하는 방식입니다.

### 🔍 정답

```java
import java.util.*;

class Solution {
    public int[][] solution(int[][] nodeinfo) {
        PriorityQueue<Node> pq = new PriorityQueue<>((o1, o2) -> o2.y - o1.y);
        for (int i = 0; i < nodeinfo.length; i++) {
            pq.add(new Node(nodeinfo[i][0], nodeinfo[i][1]));
        }
        
        Node root = pq.poll();
        Tree tree = new Tree(root.number, root.x);
        
        while (!pq.isEmpty()) {
            Node node = pq.poll();
            tree.setNode(node);
        }
        
        return new int[][]{preOrder(new ArrayList<>(), tree, nodeinfo.length), postOrder(new ArrayList<>(), tree, nodeinfo.length)};
    }
    
    public int[] preOrder(ArrayList<Integer> temp, Tree tree, int length) {
        temp.add(tree.number);
        if (tree.left != null) {
            preOrder(temp, tree.left, length);
        }
        if (tree.right != null) {
            preOrder(temp, tree.right, length);
        }
        
        return temp.stream().mapToInt(Integer::intValue).toArray();
    }
    
    public int[] postOrder(ArrayList<Integer> temp, Tree tree, int length) {    
        if (tree.left != null) {
            postOrder(temp, tree.left, length);
        }
        if (tree.right != null) {
            postOrder(temp, tree.right, length);
        }
        temp.add(tree.number);
        
        return temp.stream().mapToInt(Integer::intValue).toArray();
    }
    
    public class Tree {
        int number;
        int value;
        Tree left, right;
        
        public Tree(int number, int value) {
            this.number = number;
            this.value = value;
        }
        
        public void setNode(Node node) {
            if (node.x < this.value) {
                if (this.left == null) {
                    this.left = new Tree(node.number, node.x);
                } else {
                    this.left.setNode(node);
                }
            } else {
                if (this.right == null) {
                    this.right = new Tree(node.number, node.x);
                } else {
                    this.right.setNode(node);
                }
            }
        }
    }
    
    public static class Node {
        static int ORDER = 1;
        int number;
        int x, y;
        
        public Node(int x, int y) {
            this.number = ORDER++;
            this.x = x;
            this.y = y;
        }
    }
}
```