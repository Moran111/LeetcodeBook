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

## 437. Path Sum III



Given the `root` of a binary tree and an integer `targetSum`, return _the number of paths where the sum of the values along the path equals_ `targetSum`.

The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg)

```
Input: root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
Output: 3
Explanation: The paths that sum to 8 are shown.
```

**Example 2:**

```
Input: root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
Output: 3
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[0, 1000]`.
* `-109 <= Node.val <= 109`
* `-1000 <= targetSum <= 1000`

这个题是从上到下算prefix sum，但是从下到上做recursion，对每一层来说path sum的个数都是左边的个数 + 右边的个数 + 走到这一层符合条件的个数 （prefixSum + root.val - target的个数）。有这个target之后，就能容易的在map里找到 任意一段连续的subarray的合是target的subarray的个数了。

最后要把map还原。

```
    /*
    prefix sum: how to find continuous subarrays that sum to target
    two situation: 
    1. the subarray we want starts from the beginning of the array
    2. the subarray we want starts from somewhere in the middle
    -> how can we find how many subarray we want from case 2,
    we can have a Map<Sum, Number of times> 
    current Sum - target = previous sum
    map.get(previous sum) = how many times the previous some exists
    previous Sum + [target] = current Sum
    */
```

```
class Solution {
    //cannot across the parent root
    //but path cannot start at root or end at leaf
    public int pathSum(TreeNode root, int targetSum) {
        // prefix sum, times that prefix sum appeared
        Map<Integer, Integer> map = new HashMap<>();
        map.put(0,1);
        return helper(root, targetSum, 0, map);
    }
    
    private int helper(TreeNode root, int targetSum, 
                       int prefixSum,
                       Map<Integer, Integer> map) {
        if (root == null) {
            return 0;
        }
        // curr sum
        int currSum = prefixSum + root.val;
        // sum we want
        int prevSum = currSum - targetSum;
        int count = map.getOrDefault(prevSum, 0);
        
        // query first, then update it
        // 1, target = 0
        map.put(currSum, map.getOrDefault(currSum, 0) + 1);
        int left = helper(root.left, targetSum, currSum, map);
        int right = helper(root.right, targetSum, currSum, map);
        count = count + left + right;
        // restore the map, as the recursion goes from the bottom to the top
        map.put(currSum, map.get(currSum) - 1);
        return count;
    }
}
```

## 129. Sum Root to Leaf Numbers



You are given the `root` of a binary tree containing digits from `0` to `9` only.

Each root-to-leaf path in the tree represents a number.

* For example, the root-to-leaf path `1 -> 2 -> 3` represents the number `123`.

Return _the total sum of all root-to-leaf numbers_. Test cases are generated so that the answer will fit in a **32-bit** integer.

A **leaf** node is a node with no children.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/num1tree.jpg)

```
Input: root = [1,2,3]
Output: 25
Explanation:
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Therefore, sum = 12 + 13 = 25.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/num2tree.jpg)

```
Input: root = [4,9,0,5,1]
Output: 1026
Explanation:
The root-to-leaf path 4->9->5 represents the number 495.
The root-to-leaf path 4->9->1 represents the number 491.
The root-to-leaf path 4->0 represents the number 40.
Therefore, sum = 495 + 491 + 40 = 1026.
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[1, 1000]`.
* `0 <= Node.val <= 9`
* The depth of the tree will not exceed `10`.

dfs backtracking, 通常要有一个global的变量，用于记录整体的结果。

```
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
    int totalSum = 0;
    public int sumNumbers(TreeNode root) {
        helper(root, 0);
        return totalSum;
    }
    
    //dfs
    private void helper(TreeNode root, int sum) {
        if (root == null) {
            return;
        }
        if (root.left == null && root.right == null) {
            sum = 10 * sum + root.val;
            totalSum += sum;
            sum = (sum - root.val) / 10;
            return;
        }
        
        helper(root.left, 10 * sum + root.val);
        helper(root.right,10 * sum + root.val);
    }
}
```

## 662. Maximum Width of Binary Tree



Given the `root` of a binary tree, return _the **maximum width**of the given tree_.

The **maximum width** of a tree is the maximum **width** among all levels.

The **width** of one level is defined as the length between the end-nodes (the leftmost and rightmost non-null nodes), where the null nodes between the end-nodes that would be present in a complete binary tree extending down to that level are also counted into the length calculation.

It is **guaranteed** that the answer will in the range of a **32-bit**signed integer.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/03/width1-tree.jpg)

```
Input: root = [1,3,2,5,3,null,9]
Output: 4
Explanation: The maximum width exists in the third level with length 4 (5,3,null,9).
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/14/maximum-width-of-binary-tree-v3.jpg)

```
Input: root = [1,3,2,5,null,null,9,6,null,7]
Output: 7
Explanation: The maximum width exists in the fourth level with length 7 (6,null,null,null,null,null,7).
```

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/05/03/width3-tree.jpg)

```
Input: root = [1,3,2,5]
Output: 2
Explanation: The maximum width exists in the second level with length 2 (3,2).
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[1, 3000]`.
* `-100 <= Node.val <= 100`

需要算这个node在complete binary search tree上的哪个位置。这个题说宽度是包括中间是null的位置。所以我们可以根据每个node的col index来算每一level中的宽度。&#x20;

![BFS traversal](https://leetcode.com/problems/Figures/662/662\_bfs\_traversal.png)

We need to store the treenode & its index in the queue。总的来说还是bfs，就是需要在bfs的时候存下来col index以方便算出每一层的width

```
class Solution {
    public int widthOfBinaryTree(TreeNode root) {
        int maxWidth = 0;
        Queue<Pair<Integer, TreeNode>> queue = new LinkedList<>();
        queue.offer(new Pair(1, root));
        while(!queue.isEmpty()) {
            int size = queue.size();
            int startIndex = -1; //start index for each level
            int endIndex = -1;
            for (int i = 0; i < size; i++) {
                Pair<Integer, TreeNode> curr = queue.poll();
                if (startIndex == -1) {
                    startIndex = curr.getKey(); //2
                }
                int index = curr.getKey();
                endIndex = index; //3
                TreeNode node = curr.getValue();
                if (node.left != null) {
                    queue.offer(new Pair(2 * index, node.left));
                } 
                if (node.right != null) {
                    queue.offer(new Pair(2 * index + 1, node.right));
                } 
            }
            maxWidth = Math.max(maxWidth, endIndex - startIndex + 1); //1
        }
        return maxWidth;
    }
}
```

## 199. Binary Tree Right Side View



Given the `root` of a binary tree, imagine yourself standing on the **right side**of it, return _the values of the nodes you can see ordered from top to bottom_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)

```
Input: root = [1,2,3,null,5,null,4]
Output: [1,3,4]
```

**Example 2:**

```
Input: root = [1,null,3]
Output: [1,3]
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

```
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        if (root == null) {
            return new ArrayList<>();
        }
        List<Integer> res = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        
        int index = 0;
        while(!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                TreeNode curr = queue.poll();
                res.add(index, curr.val);
                if (curr.left != null) {
                    queue.offer(curr.left);
                }
                if (curr.right != null) {
                    queue.offer(curr.right);
                }
            }
            index++;
        }
        
        return res.subList(0, index);
    }
}
```

每一层里只加最后一个node，可以有一个prev和curr node，然后在每一层的时候找到最后一个node，加进去, 或者当i = levelLength-1 （是最后一个node的时候） 的时候加入到res里

```
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        if (root == null) return new ArrayList<Integer>();
        
        ArrayDeque<TreeNode> queue = new ArrayDeque(){{ offer(root); }};
        List<Integer> rightside = new ArrayList();
        
        while (!queue.isEmpty()) {
            int levelLength = queue.size();

            for(int i = 0; i < levelLength; ++i) {
                TreeNode node = queue.poll();
                // if it's the rightmost element
                if (i == levelLength - 1) { // last index
                    rightside.add(node.val);    
                }

                // add child nodes in the queue
                if (node.left != null) {
                    queue.offer(node.left);    
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
        }
        return rightside;
    }
}
```





