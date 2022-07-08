# Medium

## 236. Lowest Common Ancestor of a Binary Tree



Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest\_common\_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
Output: 3
Explanation: The LCA of nodes 5 and 1 is 3.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
Input: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
Output: 5
Explanation: The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.
```

**Example 3:**

```
Input: root = [1,2], p = 1, q = 2
Output: 1
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[2, 105]`.
* `-109 <= Node.val <= 109`
* All `Node.val` are **unique**.
* `p != q`
* `p` and `q` will exist in the tree.

&#x20;分情况： 如果p和分别在左右或者右左的话，那么lowest common anscester就是他们的root，但是如果p和q在同一边的话，如果p在上面就lca就是p，q在上面就是q。怎么能知道p在上面还是q在上面呢？递归的时候可以返回我们找到的node，最后一个找到的就是比较上面的那个。我们递归返回的是parent。

```
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // root == null
        if (root == null) {
            return null;
        }
        // if we found p or q
        if (root == p || root == q) {
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        // if p or q in the left subtree and right subtree
        if (left != null && right != null) {
            return root;
        } else if (left != null) { // if it only in the left subtree
            return left;
        } else { // if it only in the right subtree
            return right;
        }
    }
}
```

## 113. Path Sum II

Given the `root` of a binary tree and an integer `targetSum`, return _all **root-to-leaf** paths where the sum of the node values in the path equals_ `targetSum`_. Each path should be returned as a list of the node **values**, not node references_.

A **root-to-leaf** path is a path starting from the root and ending at any leaf node. A **leaf** is a node with no children.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: [[5,4,11,2],[5,8,4,5]]
Explanation: There are two paths whose sum equals targetSum:
5 + 4 + 11 + 2 = 22
5 + 8 + 4 + 5 = 22
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

```
Input: root = [1,2,3], targetSum = 5
Output: []
```

**Example 3:**

```
Input: root = [1,2], targetSum = 0
Output: []
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[0, 5000]`.
* `-1000 <= Node.val <= 1000`
* `-1000 <= targetSum <= 1000`

注意sum是要在return前减，还是在return后减。为什么要同时check root ==null 和是不是leaf呢？因为有可能有root的一边是空的情况，和root是leaf且sum = 0。

```
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> result = new ArrayList<>();
        dfs(root, targetSum, new ArrayList<>(), result);
        return result;
    }
    
    private void dfs(TreeNode root, int sum, 
                     List<Integer> res, List<List<Integer>> result) {
        if (root == null) {
            return;
        }
        sum -= root.val; 
        if (root.left == null && root.right == null) {
            if (sum == 0) {
                res.add(root.val);
                result.add(new ArrayList(res));
                res.remove(res.size() - 1);
            }
            return; // return -> 返回上次层的sum和res
        }
        res.add(root.val);
        dfs(root.left, sum, res, result);
        dfs(root.right, sum, res, result);
        res.remove(res.size()-1);
    }
}
```



