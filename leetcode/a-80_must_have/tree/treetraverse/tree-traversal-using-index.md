# Tree Traversal Using Index

## 662. Maximum Width of Binary Tree

Given the `root` of a binary tree, return _the **maximum width** of the given tree_.

The **maximum width** of a tree is the maximum **width** among all levels.

The **width** of one level is defined as the length between the end-nodes (the leftmost and rightmost non-null nodes), where the null nodes between the end-nodes are also counted into the length calculation.

It is **guaranteed** that the answer will in the range of **32-bit** signed integer.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/03/width1-tree.jpg)

```
Input: root = [1,3,2,5,3,null,9]
Output: 4
Explanation: The maximum width existing in the third level with the length 4 (5,3,null,9).
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/05/03/width2-tree.jpg)

```
Input: root = [1,3,null,5,3]
Output: 2
Explanation: The maximum width existing in the third level with the length 2 (5,3).
```

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/05/03/width3-tree.jpg)

```
Input: root = [1,3,2,5]
Output: 2
Explanation: The maximum width existing in the second level with the length 2 (3,2).
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[1, 3000]`.
* `-100 <= Node.val <= 100`

for this question, for each level, we need to know the left boundary (non null)  and right (non null)boundary in this level -> we need to know the column index in this level, we can use right column index - left column index to know how many nodes we have in this level

![](<../../../../.gitbook/assets/Screen Shot 2022-01-02 at 10.41.41 PM (1).png>)

column index start from 1. The left node tree is 2 \* its index. right node is 2 \* its index + 1. This is how to calculate for a full binary tree. For this question, even though it is not a full binary tree, it doesn't matter. We want to calculate the diff.&#x20;

```
class Solution {
    public int widthOfBinaryTree(TreeNode root) {
        // each level of the tree can have the index, 
        // we can use the index of column to calculate the width
        
        int maxWidth = 0;
        Queue<Pair<TreeNode, Integer>> queue = new LinkedList<>();
        queue.offer(new Pair(root, 1));
        
        while(!queue.isEmpty()) {
            int size = queue.size();
            int currWidth = 0;
            Pair<TreeNode, Integer> firstItem = queue.peek();
            Pair<TreeNode, Integer> lastItem = null;
            for (int i = 0; i < size; i++) {
                Pair<TreeNode, Integer> curr = queue.poll();
                lastItem = curr;
                TreeNode currNode = curr.getKey();
                if (currNode.left != null) {
                    queue.offer(new Pair(currNode.left, 2 * curr.getValue()));
                }
                if (currNode.right != null) {
                    queue.offer(new Pair(currNode.right, 2 * curr.getValue() + 1));
                }
            }
            currWidth = lastItem.getValue() - firstItem.getValue() + 1;
            maxWidth = Math.max(maxWidth, currWidth);
        }
        return maxWidth;
    }
}
```
