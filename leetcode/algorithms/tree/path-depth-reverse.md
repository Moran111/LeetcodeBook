# Path,Depth,Reverse

101\. Symmetric Tree

Given the `root` of a binary tree, _check whether it is a mirror of itself_ (i.e., symmetric around its center).

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

```
Input: root = [1,2,2,3,4,4,3]
Output: true
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)

```
Input: root = [1,2,2,null,3,null,3]
Output: false
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[1, 1000]`.
* `-100 <= Node.val <= 100`

&#x20;

**Follow up:** Could you solve it both recursively and iteratively?

考虑一下这个题 在每一层的时候需要比较left和right subtree的顶点，所以helper的时候需要左右两顶点。每层继续往下递归的话，需要找left.left and right.right && left.right and right.left.

```
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return helper(root, root);
    }
    
    // there roots have the same value
    // left.left = right.right && left.right = right.left
    private boolean helper(TreeNode left, TreeNode right) {
        if (left == null && right == null) {
            return true;
        }
        // if one of them is null
        if (left == null || right == null) {
            return false;
        }
        
        boolean leftSub = helper(left.left, right.right);
        boolean rightSub = helper(left.right, right.left);
        return (left.val == right.val) && leftSub && rightSub;
    }
}
```

