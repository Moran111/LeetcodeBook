# Binary Tree Level Order Traversal

Description

Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

* The first data is the root node, followed by the value of the left and right son nodes, and "#" indicates that there is no child node.
* The number of nodes does not exceed 20.

Example

**Example 1:**

Input:

```
tree = {1,2,3}
```

Output:

```
[[1],[2,3]]
```

Explanation:

```
   1 
  / \ 
 2   3 
```

it will be serialized {1,2,3}\
**Example 2:**

Input:

```
tree = {1,#,2,3} 
```

Output:

```
[[1],[2],[3]] 
```

Explanation:

```
1 
 \ 
  2 
 / 
3 
```

it will be serialized {1,#,2,3}

Challenge

Challenge 1: Using only 1 queue to implement it.

Challenge 2: Use BFS algorithm to do it.



```
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */

public class Solution {
    /**
     * @param root: A Tree
     * @return: Level order a list of lists of integer
     */
    public List<List<Integer>> levelOrder(TreeNode root) {
        // write your code here
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while(!queue.isEmpty()) {
            List<Integer> temp = new ArrayList<>();
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode curr = queue.poll();
                temp.add(curr.val);
                if (curr.left != null) {
                    queue.add(curr.left);
                }
                if (curr.right != null) {
                    queue.add(curr.right);
                }   
            }
            res.add(temp);
        }
        return res;
    }


}
```
