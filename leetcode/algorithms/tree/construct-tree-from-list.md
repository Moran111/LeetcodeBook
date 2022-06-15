# Construct Tree From List

## 108. Convert Sorted Array to Binary Search Tree



Given an integer array `nums` where the elements are sorted in **ascending order**, convert _it to a **height-balanced** binary search tree_.

A **height-balanced** binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/18/btree1.jpg)

```
Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted:
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/18/btree.jpg)

```
Input: nums = [1,3]
Output: [3,1]
Explanation: [1,null,3] and [3,1] are both height-balanced BSTs.
```

&#x20;

**Constraints:**

* `1 <= nums.length <= 104`
* `-104 <= nums[i] <= 104`
* `nums` is sorted in a **strictly increasing** order.

Construct the Binary Search Tree from Sorted Array

```
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        return helper(nums, 0, nums.length-1);
    }
    
    public TreeNode helper(int[] nums, int s, int e) {
        if (s > e) {
            return null;
        }
        if (s == e) {
            return new TreeNode(nums[s]);
        }
        
        int mid = s + (e - s)/2;
        TreeNode node = new TreeNode(nums[mid]);
        
        TreeNode left = helper(nums, s, mid-1);
        TreeNode right = helper(nums, mid + 1, e);
        
        if (left != null) {
            node.left = left;
        }
        if (right != null) {
            node.right = right;
        }
        return node;
    }
}
```

## 105. Construct Binary Tree from Preorder and Inorder Traversal



105\. Construct Binary Tree from Preorder and Inorder TraversalMedium8733239Add to ListShare

Given two integer arrays `preorder` and `inorder` where `preorder` is the preorder traversal of a binary tree and `inorder` is the inorder traversal of the same tree, construct and return _the binary tree_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: preorder = [-1], inorder = [-1]
Output: [-1]
```

&#x20;

**Constraints:**

* `1 <= preorder.length <= 3000`
* `inorder.length == preorder.length`
* `-3000 <= preorder[i], inorder[i] <= 3000`
* `preorder` and `inorder` consist of **unique** values.
* Each value of `inorder` also appears in `preorder`.
* `preorder` is **guaranteed** to be the preorder traversal of the tree.
* `inorder` is **guaranteed** to be the inorder traversal of the tree.

找出root的index，然后递归的建立左边的tree和右边的tree，然后从左边递归找root 右边递归找root，继续递归下去

在preorder里，root + （左边tree的长度）= right tree root

```
/*
root + (left subtree length = rootIdx - start) + 1
 0
[3,9,20,15,7]
[9,3,15,20,7]
   1
   rootIdx
*/
```

```
class Solution {
    int rootIndex = 0;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        // preorder root, left, right
        // inorder  left root right
        return helper(preorder, inorder, 0, inorder.length - 1);
    }
    
    public TreeNode helper(int[] preorder, int[] inorder, int s, int e) {
        if (rootIndex >= preorder.length || s > e) {
            return null;
        }
        
        int rootVal = preorder[rootIndex];
        TreeNode root = new TreeNode(rootVal);
        rootIndex++;
        
        // inorder -> unique
        int index = -1;
        for (int i = 0; i < inorder.length; i++) {
            if (inorder[i] == rootVal) {
                index = i;
            }
        }
        TreeNode left = helper(preorder, inorder, s, index - 1);
        TreeNode right = helper(preorder, inorder, index + 1, e);
        
        if (left != null) {
            root.left = left;
        }
        if (right != null) {
            root.right = right;
        }
        return root;
    }
```

```
public TreeNode buildTree(int[] preorder, int[] inorder) {
    return helper(0, 0, inorder.length - 1, preorder, inorder);
}

public TreeNode helper(int preStart, int inStart, int inEnd, int[] preorder, int[] inorder) {
    if (preStart > preorder.length - 1 || inStart > inEnd) {
        return null;
    }
    TreeNode root = new TreeNode(preorder[preStart]);
    int inIndex = 0; // Index of current root in inorder
    for (int i = inStart; i <= inEnd; i++) {
        if (inorder[i] == root.val) {
            inIndex = i;
        }
    }
    root.left = helper(preStart + 1, inStart, inIndex - 1, preorder, inorder);
    root.right = helper(preStart + inIndex - inStart + 1, inIndex + 1, inEnd, preorder, inorder);
    return root;
}
```

## 106. Construct Binary Tree from Inorder and Postorder Traversal



Given two integer arrays `inorder` and `postorder` where `inorder` is the inorder traversal of a binary tree and `postorder` is the postorder traversal of the same tree, construct and return _the binary tree_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
Input: inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
Output: [3,9,20,null,null,15,7]
```

**Example 2:**

```
Input: inorder = [-1], postorder = [-1]
Output: [-1]
```

&#x20;

**Constraints:**

* `1 <= inorder.length <= 3000`
* `postorder.length == inorder.length`
* `-3000 <= inorder[i], postorder[i] <= 3000`
* `inorder` and `postorder` consist of **unique** values.
* Each value of `postorder` also appears in `inorder`.
* `inorder` is **guaranteed** to be the inorder traversal of the tree.
* `postorder` is **guaranteed** to be the postorder traversal of the tree.

```
class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        // inorder left root right
        //              ridx       e
        // postorder        left            right  root
        //           (left root)     (right root)
        
        return helper(postorder.length - 1, 0, inorder.length - 1, inorder, postorder);
    }
    
    private TreeNode helper(int rootIndex, int s, int e, int[] inorder,
                           int[] postorder) {
        if (s > e || rootIndex < 0) {
            return null;
        }
        
        int rootVal = postorder[rootIndex];
        TreeNode root = new TreeNode(rootVal);
        
        int idx = -1;
        for (int i = 0; i < inorder.length; i++) {
            if (inorder[i] == rootVal) {
                idx = i;
            }
        }
        
        TreeNode left = helper(rootIndex - (e - idx) - 1, s, idx - 1, inorder, postorder);
        TreeNode right = helper(rootIndex - 1, idx + 1, e, inorder, postorder);
        
        if (left != null) {
            root.left = left;
        }
        if (right != null) {
            root.right = right;
        }
        return root;
    }
}
```

## 889. Construct Binary Tree from Preorder and Postorder Traversal



Given two integer arrays, `preorder` and `postorder` where `preorder`is the preorder traversal of a binary tree of **distinct** values and `postorder` is the postorder traversal of the same tree, reconstruct and return _the binary tree_.

If there exist multiple answers, you can **return any** of them.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/24/lc-prepost.jpg)

```
Input: preorder = [1,2,4,5,3,6,7], postorder = [4,5,2,6,7,3,1]
Output: [1,2,3,4,5,6,7]
```

**Example 2:**

```
Input: preorder = [1], postorder = [1]
Output: [1]
```

&#x20;

**Constraints:**

* `1 <= preorder.length <= 30`
* `1 <= preorder[i] <= preorder.length`
* All the values of `preorder` are **unique**.
* `postorder.length == preorder.length`
* `1 <= postorder[i] <= postorder.length`
* All the values of `postorder` are **unique**.
* It is guaranteed that `preorder` and `postorder` are the preorder traversal and postorder traversal of the same binary tree.

和前面inorder list的思路大体上是一样的，但是略有不同，不同主要在于 inorder list本身就是left root right，很方便找到left和right的边界

但是这个题只给了preorder和postorder

```
preorder : root left() right()
postorder: ()left ()right root
```

但我们用preorder去做递归，要知道left开始的点- root + 1，left结束的点 - left开始得点+left()的长度，自然也就知道了right开始的点和right结束的点

怎么知道left()的长度呢？ 可以用postorder来找, 在preorder中left的4 -> 对应在postorder中就是()left，left的结束的点，从preStart + (leftIndx - postStart) + 1, 就是左边的个数 -》用root + 左边的个数，就是preorder中左边递归的边界，右边开始的点就是preStart + (leftIndx - postStart) + 2

postorder的边界的长度要和preorder递归时的长度保持一致

```
preorder =  [1,2,4,5,3,6,7], 
postorder = [4,5,2,6,7,3,1]

=>
[1,  2,4,5,  3,6,7]
[    4,5,2,  6,7,3,  1]
```

```
class Solution {
    public TreeNode constructFromPrePost(int[] preorder, int[] postorder) {
        // preorder   root left(root left() right()) right()
        // postorder  ()left ()right root
        // [1,2,4,5,3,6,7]
        // [4,5,2,6,7,3,1]
        
        return helper(preorder, 0, preorder.length - 1, 
                      postorder, 0, postorder.length - 1);
    }
    
    private TreeNode helper(int[] preorder, int preStart, int preEnd, 
                            int[] postorder, int postStart, int postEnd) {
        // find the start of the left subtree
        // use pre order do recursion
        if (preStart > preEnd) {
            return null;
        }
        // must have it, or have index out of bound
        if (preStart == preEnd) {
            return new TreeNode(preorder[preStart]);
        }
        TreeNode root = new TreeNode(preorder[preStart]);
        // find the start of the left in postorder 
        int index = postStart;
        while(index <= postEnd && postorder[index] != preorder[preStart + 1]) {
            index++;
        }
        
        //index -> 2
        // preStart + length of left subtree 
        // reduce size of preorder and postorder
        // postorder should keep same size with preorder, need to use it to calculate 
        // the number of left subtree 
        root.left = helper(preorder, preStart + 1, preStart + (index - postStart) + 1, 
                              postorder, postStart, index);
        root.right = helper(preorder, preStart + (index - postStart) + 2, preEnd,
                               postorder, index + 1, postEnd - 1);
        
        
        return root;
    }
}
```



