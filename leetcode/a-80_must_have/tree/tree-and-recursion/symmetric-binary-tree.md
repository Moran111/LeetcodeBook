# Symmetric Binary Tree

Description

Given a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

Example

**Example 1:**

```
Input: {1,2,2,3,4,4,3}
Output: true
Explanation:
         1
        / \
       2   2
      / \ / \
      3 4 4 3

is a symmetric binary tree.
```

**Example 2:**

```
Input: {1,2,2,#,3,#,3}
Output: false
Explanation:
         1
        / \
        2  2
        \   \
         3   3

is not a symmetric binary tree.
```

Challenge

Can you solve it both recursively and iteratively?

和平时写的递归不太一样，要考虑每一层的时候干了什么。比如说2，2。要检查left 和right是不是一样的，还要检查left.left == right.right && left.right == right.left. 要检查两个东西。需要传进去2个参数。 下一层就是4个了。

平时的递归有两种，1. 左子树返回结果 右子树返回结果，处里这两个结果，返回新的结果。2. path东西进去，从上到下，下面的处理上面path进去的东西。257. Binary Path Sum: [https://leetcode.com/problems/binary-tree-paths/](https://leetcode.com/problems/binary-tree-paths/)

```
         1
        / \
       2   2
      / \ / \
      3 4 4 3
    /\      /\
   5 6      6 
```

```
public class Solution {
    /**
     * @param root: the root of binary tree.
     * @return: true if it is a mirror of itself, or false.
     */
    public boolean isSymmetric(TreeNode root) {
        // write your code here
        if (root == null) {
            return true;
        }
        return helper(root.left, root.right);
    }

    private boolean helper(TreeNode leftNode, TreeNode rightNode) {
        if (leftNode == null && rightNode == null) {
            return true;
        }
        if (leftNode == null || rightNode == null) {
            return false;
        }
        if (leftNode.val != rightNode.val) {
            return false;
        } 
        return helper(leftNode.left, rightNode.right) && helper(leftNode.right, rightNode.left);
    }
}
```
