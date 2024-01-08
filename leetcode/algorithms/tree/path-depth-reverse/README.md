# Path,Depth,Reverse

## 101. Symmetric Tree

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

## 226. Invert Binary Tree



Given the `root` of a binary tree, invert the tree, and return _its root_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```
Input: root = [4,2,7,1,3,6,9]
Output: [4,7,2,9,6,3,1]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

```
Input: root = [2,1,3]
Output: [2,3,1]
```

**Example 3:**

```
Input: root = []
Output: []
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[0, 100]`.
* `-100 <= Node.val <= 100`

这个就只会放root进入helper 函数，因为我们在每一层就需要一个root就够了

```
class Solution {
    public TreeNode invertTree(TreeNode root) {
        return helper(root);
    }
    
    private TreeNode helper(TreeNode root) {
        if (root == null) {
            return null;
        }
        
        TreeNode leftNode = helper(root.left);
        TreeNode rightNode = helper(root.right);
        
        root.left = rightNode;
        root.right = leftNode;
        
        return root;
    }
}
```

## 543. Diameter of Binary Tree



Given the `root` of a binary tree, return _the length of the **diameter**of the tree_.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```
Input: root = [1,2,3,4,5]
Output: 3
Explanation: 3 is the length of the path [4,2,1,3] or [5,2,1,3].
```

**Example 2:**

```
Input: root = [1,2]
Output: 1
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[1, 104]`.
* `-100 <= Node.val <= 100`

```
class Solution {
    int max = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        helper(root);
        return max - 1;
    }
    
    // return the max depth of left or right sub subtree
    public int helper(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int left = helper(root.left);
        int right = helper(root.right);
        
        max = Math.max(max, left + 1 + right);
        
        return Math.max(left, right) + 1;
    }
    
    // 3 type of diameter 
    // diameter = left subtree max diameter + 1 + right subtree max diameter 
    // need to keep a global max diameter 
}
```

## 257. Binary Tree Paths



Given the `root` of a binary tree, return _all root-to-leaf paths in **any order**_.

A **leaf** is a node with no children.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

```
Input: root = [1,2,3,null,5]
Output: ["1->2->5","1->3"]
```

**Example 2:**

```
Input: root = [1]
Output: ["1"]
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[1, 100]`.
* `-100 <= Node.val <= 100`

典型的dfs，最后希望加1 -》2 -〉 5. 5 之后并不加-》。所以我们可以可以肯定什么时候加入到res 里呢？肯定得是等到是leaf node的时候才能加入到res里，如果这个时候加入到res里，那么最后一个node就还没加呢。正合适

```
class Solution {
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> res = new ArrayList<>();
        dfs(root, "", res);
        return res;
    }
    
    // dfs 
    public void dfs(TreeNode root, String sb, List<String> res) {
        if (root == null) {
            return;
        }
        if (root.left == null && root.right == null) {
            res.add(sb + root.val);
            return;
        }
        
        sb += root.val + "->";
        dfs(root.left, sb, res);
        dfs(root.right, sb, res);
    }
}
```
