# Kth Smallest Integer in BST

Given the `root` of a binary search tree, and an integer `k`, return the `kth`smallest value (**1-indexed**) in the tree.

A **binary search tree** satisfies the following constraints:&#x20;

* The left subtree of every node contains only nodes with keys **less than**the node's key.
* The right subtree of every node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees are also binary search trees.

**Example 1:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/02eca3db-f72f-4277-7134-faec4f02e500/public)

```java
Input: root = [2,1,3], k = 1

Output: 1
```

**Example 2:**

![](https://imagedelivery.net/CLfkmk9Wzy8_9HRyug4EVA/dca6c42d-2327-4036-f7f2-3e99d8203100/public)

```java
Input: root = [4,3,5,2,null], k = 4

Output: 5
```

**Constraints:**

* `1 <= k <= The number of nodes in the tree <= 1000`.
* `0 <= Node.val <= 1000`

My First Version Code:&#x20;

```
class Solution {
    int res = -1;
    public int kthSmallest(TreeNode root, int k) {
        // inorder traverse
        dfs(root, k);
        return res;
    }

    private void dfs(TreeNode root, int k) {
        if (root == null) return;
        dfs(root.left, k--);           <<<< not correct
        System.out.println(k + " " + root.val);
        if (k == 0) {
            res = root.val;
            return;
        }
        dfs(root.right, k--);          <<<< not correct
    }
}
```

I cannot pass k--  here. Each recursive call gets its **own copy of `k`**, and the changes made to `k` inside recursive calls are **not shared**. This is because Java is **pass-by-value** (even for primitives).

```
You're passing k--, which means:
The value of k is passed before it's decremented
The decremented k does not affect the rest of the recursion
```

I need to make **make `k` a class variable**, just like `res`.

```
class Solution {
    int res = -1;
    int count = 0;
    public int kthSmallest(TreeNode root, int k) {
        // inorder traverse
        count = k;
        dfs(root);
        return res;
    }

    private void dfs(TreeNode root) {
        if (root == null) return;
        dfs(root.left);
        count--;
        if (count == 0) {
            res = root.val;
            return;
        }
        dfs(root.right);
    }
}
```
