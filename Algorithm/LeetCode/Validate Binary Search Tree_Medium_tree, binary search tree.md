# [문제링크](https://leetcode.com/problems/validate-binary-search-tree)

## 📝 문제

Given the `root` of a binary tree, _determine if it is a valid binary search tree (BST)_.

A **valid BST** is defined as follows:

- The left 
    
    subtree
    
     of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

**Input:** root = [2,1,3]
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg)

**Input:** root = [5,1,4,null,null,3,6]
**Output:** false
**Explanation:** The root node's value is 5 but its right child's value is 4.

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-231 <= Node.val <= 231 - 1`


---

### 💡 풀이

- 이진탐색트리의 조건을 만족하는지 여부를 판단하는 문제입니다.
- 모든 부모노드는 왼쪽 자식 노드보다 커야 하고 오른쪽 자식 노드보다는 작아야 합니다. 이러한 조건은 하위트리에서도 마찬가지여야 합니다.
- 예를 들어서 현재 노드의 오른쪽 자식이라면 오른쪽 자식의 왼쪽 노드는 오른쪽 자식보다 작아야 하지만 현재 노드보다는 커야 합니다.
- 따라서 단순히 부모 노드와 왼쪽 노드, 오른쪽 노드만을 비교해서는 안 되겠죠?!
- 이 조건을 해결하기 위해서는 왼쪽 노드를 탐색할 때는 부모 노드보다 작은지를 확인해야할 뿐만 아니라 이전 부모 노드의 값보다 작은지도 확인해야 하므로 이전 부모 노드의 값을 max로 갱신해가며 비교하였습니다.
- 오른쪽 노드를 탐색할 때는 반대로 부모 노드보다 큰 지 확인할 뿐만 아니라 이전 부모 노드보다도 큰 지를 확인하기 위해 min 값을 갱신하며 비교하였습니다.


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
    public boolean isValidBST(TreeNode root) {
        return checkValid(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean checkValid(TreeNode node, long min, long max) {
        if (node == null) return true;
        if (node.val <= min) return false;
        if (node.val >= max) return false;

        return checkValid(node.left, min, node.val) && checkValid(node.right, node.val, max);
    }
}
```