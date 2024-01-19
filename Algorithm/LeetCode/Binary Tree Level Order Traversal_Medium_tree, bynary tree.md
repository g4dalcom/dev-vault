# [문제링크](https://leetcode.com/problems/binary-tree-level-order-traversal/)

## 📝 문제

Given the `root` of a binary tree, return _the level order traversal of its nodes' values_. (i.e., from left to right, level by level).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

**Input:** root = [3,9,20,null,null,15,7]
**Output:** [[3],[9,20],[15,7]]

**Example 2:**

**Input:** root = [1]
**Output:** [[1]]

**Example 3:**

**Input:** root = []
**Output:** []

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-1000 <= Node.val <= 1000`


---

### 💡 풀이

- result 리스트의 각 인덱스를 트리의 깊이로 사용하였습니다.
- result.get(0) 은 루트 노드의 value를 담은 리스트고, result.get(1) 은 루트 노드와 1만큼 떨어진 노드의 value들이 담긴 리스트입니다.
- 이러한 거리를 알기 위해 depth 배열을 사용해서 매번 재귀할 때마다 depth+1을 해주었습니다.
- 또한 result.size() 가 현재 depth보다 크지 않다면 해당 깊이는 처음 방문하는 것이라고 볼 수 있기 때문에 새로운 리스트를 생성하고 그렇지 않다면 기존에 있던 리스트에 현재 node.val 값을 추가해주는 식으로 구현하였습니다!


### 🔍 정답

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    static List<List<Integer>> result;

    public List<List<Integer>> levelOrder(TreeNode root) {
        result = new ArrayList<>();
        postOrder(root, 0);

        return result;
    }

    public void postOrder(TreeNode node, int depth) {
        if (node == null) return;

        if (result.size() <= depth) result.add(new ArrayList<>());
        result.get(depth).add(node.val);

        if (node.left != null) postOrder(node.left, depth+1);
        if (node.right != null) postOrder(node.right, depth+1);
    }
}
```