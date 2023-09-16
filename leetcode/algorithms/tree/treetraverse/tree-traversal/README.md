# Tree Traversal

## 94 Binary Tree Inorder Traversal

Iterative:&#x20;

add all left side node first, then for each stack pop, if they have right node, we add right node

```
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque(); 
        
        TreeNode node = root;
        while(node != null) { 
            stack.push(node);
            node = node.left;
        }
        // 1 
        while(!stack.isEmpty()) {
            TreeNode curr = stack.pop();
            res.add(curr.val);
            TreeNode currRight = curr.right; 
            while(currRight != null) {
                stack.push(currRight);
                currRight = currRight.left;
            }
        }
        
        return res;
    }
}
```

Recursion:

```
class Solution {
    List<Integer> res = new ArrayList<>();
    public List<Integer> inorderTraversal(TreeNode root) {
        traverse(root);
        return res;
    }
    
    private void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        traverse(root.left);
        res.add(root.val);
        traverse(root.right);
    }
}
```

## 144 Binary Tree Preorder Traversal

Iterative

root - left - right, for each node in stack, add it right node and add its left node to stack

```
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Deque<TreeNode> stack = new ArrayDeque<>();
        
        TreeNode node = root;
        stack.push(node);
        
        while(!stack.isEmpty()) {
            TreeNode curr = stack.pop();
            res.add(curr.val);
            // add right first, will be pop later
            if (curr.right != null) {
                stack.push(curr.right);
            }
            
            if (curr.left != null) {
                stack.push(curr.left);
            }
        }
        
        return res;
    }
}
```

Recursion

```
class Solution {
     List<Integer> res = new ArrayList<>();
    public List<Integer> preorderTraversal(TreeNode root) {
        traverse(root);
        return res;
    }
    
    private void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        res.add(root.val);
        traverse(root.left);
        traverse(root.right);
    }
}
```

## 145. Binary Tree Postorder Traversal

Iterative: left - right - root&#x20;

Just like in order traversal, we add all left side node to stack. If curr's right subtree is not added, then we add its right subtree. How to know if the right subtree is visited? if curr.right == lastNode, the lastNode is added to stack, we get its right value. else, we added right subtree to stack

Recursion&#x20;

```
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque(); 
        
        TreeNode lastNode = null;
        TreeNode node = root;
        while(node != null) {
            stack.push(node);
            node = node.left;
        }
        
        while(!stack.isEmpty()) {
            TreeNode curr = stack.peek();
            TreeNode currRight = curr.right;
            // curr's right subtree is visited      
            if (currRight == null || currRight == lastNode) {
                lastNode = stack.pop();
                res.add(lastNode.val);
            } else {
                 while(currRight != null) {
                    stack.push(currRight);
                    currRight = currRight.left;
                }
            }  
        }
        return res;
    }
}
```

Recursion

```
class Solution {
    List<Integer> res = new ArrayList<>();
    public List<Integer> postorderTraversal(TreeNode root) {
        traverse(root);
        return res;
    }
    
    private void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        traverse(root.left);
        traverse(root.right);
        res.add(root.val);
    }
}
```

## 240. Kth Smallest Element in a BST

Given the `root` of a binary search tree, and an integer `k`, return _the_ `kth` _smallest value (**1-indexed**) of all the values of the nodes in the tree_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg)

<pre><code><strong>Input: root = [3,1,4,null,2], k = 1
</strong><strong>Output: 1
</strong></code></pre>

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg)

<pre><code><strong>Input: root = [5,3,6,2,4,null,null,1], k = 3
</strong><strong>Output: 3
</strong></code></pre>

&#x20;

**Constraints:**

* The number of nodes in the tree is `n`.
* `1 <= k <= n <= 104`
* `0 <= Node.val <= 104`

&#x20;

**Follow up:** If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the kth smallest frequently, how would you optimize?

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
    public int kthSmallest(TreeNode root, int k) {
        // binary search tree, inorder traverse using stack, k - 1
        Stack<TreeNode> stack = new Stack<>();
        while(root != null) {
            stack.push(root);
            root = root.left;
        }
        // 5 3 2 1
        while (!stack.isEmpty()) {
            TreeNode curr = stack.pop();
            k = k-1;
            if (k == 0) {
                return curr.val;
            }
            // put right's left
            curr = curr.right;
            while(curr != null) {
                stack.push(curr);
                curr = curr.left;
            }
        }
        return -1;
    }
}
```

## 285. Inorder Successor in BST

Given the `root` of a binary search tree and a node `p` in it, return _the in-order successor of that node in the BST_. If the given node has no in-order successor in the tree, return `null`.

The successor of a node `p` is the node with the smallest key greater than `p.val`.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/23/285\_example\_1.PNG)

<pre><code><strong>Input: root = [2,1,3], p = 1
</strong><strong>Output: 2
</strong><strong>Explanation: 1's in-order successor node is 2. Note that both p and the return value is of TreeNode type.
</strong></code></pre>

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/23/285\_example\_2.PNG)

<pre><code><strong>Input: root = [5,3,6,2,4,null,null,1], p = 6
</strong><strong>Output: null
</strong><strong>Explanation: There is no in-order successor of the current node, so the answer is null.
</strong></code></pre>

&#x20;

Given the `root` of a binary search tree and a node `p` in it, return _the in-order successor of that node in the BST_. If the given node has no in-order successor in the tree, return `null`.

The successor of a node `p` is the node with the smallest key greater than `p.val`.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/23/285\_example\_1.PNG)

<pre><code><strong>Input: root = [2,1,3], p = 1
</strong><strong>Output: 2
</strong><strong>Explanation: 1's in-order successor node is 2. Note that both p and the return value is of TreeNode type.
</strong></code></pre>

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/23/285\_example\_2.PNG)

<pre><code><strong>Input: root = [5,3,6,2,4,null,null,1], p = 6
</strong><strong>Output: null
</strong><strong>Explanation: There is no in-order successor of the current node, so the answer is null.
</strong></code></pre>

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[1, 104]`.
* `-105 <= Node.val <= 105`
* All Nodes will have unique values.

need to know the last TreeNode&#x20;

The successor of a `node` is the node with the smallest key greater than `node.val`.

```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        // TreeNode lastNode, inorder traverse
        TreeNode last = null;
        Stack<TreeNode> stack = new Stack<>();
        while(root != null) {
            stack.push(root);
            root = root.left;
        }

        while(!stack.isEmpty()) {
            TreeNode curr = stack.pop();
            if (last == p) {
                return curr;   
            }
            last = curr;
            curr = curr.right;
            while(curr != null) {
                stack.push(curr);
                curr = curr.left;
            }
        }
        return null;
    }
}
```

## 510. Inorder Successor in BST II

Given a `node` in a binary search tree, return _the in-order successor of that node in the BST_. If that node has no in-order successor, return `null`.

The successor of a `node` is the node with the smallest key greater than `node.val`.

You will have direct access to the node but not to the root of the tree. Each node will have a reference to its parent node. Below is the definition for `Node`:

```
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
}
```

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/23/285\_example\_1.PNG)

<pre><code><strong>Input: tree = [2,1,3], node = 1
</strong><strong>Output: 2
</strong><strong>Explanation: 1's in-order successor node is 2. Note that both the node and the return value is of Node type.
</strong></code></pre>

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/23/285\_example\_2.PNG)

<pre><code><strong>Input: tree = [5,3,6,2,4,null,null,1], node = 6
</strong><strong>Output: null
</strong><strong>Explanation: There is no in-order successor of the current node, so the answer is null.
</strong></code></pre>

**Constraints:**

* The number of nodes in the tree is in the range `[1, 104]`.
* `-105 <= Node.val <= 105`
* All Nodes will have unique values.

&#x20;

**Follow up:** Could you solve it without looking up any of the node's values?

* Node has no right child, then its successor is somewhere upper in the tree. To find the successor, go up till the node that is _left_ child of its parent. The answer is the parent. Beware that there could be no successor (= null successor) in such a situation.

![pac](https://leetcode.com/problems/inorder-successor-in-bst-ii/solutions/306596/Figures/510/case.png)



![fic](https://leetcode.com/problems/inorder-successor-in-bst-ii/solutions/306596/Figures/510/casenull.png)

```
/*
// Definition for a Node.
class Node {
    public int val;
    public Node left;
    public Node right;
    public Node parent;
};
*/

class Solution {
    public Node inorderSuccessor(Node node) {
        // either right exists or not.
        // right exist, right's most left
        // right not exist, go up till the node that is left child of its parent.

        // the successor is somewhere lower in the right subtree
        if (node.right != null) {
            node = node.right;
            while(node.left != null) {
                node = node.left;
            }
            return node;
        }

        // the successor is somewhere upper in the tree
        while(node.parent != null && node.parent.right == node) {
            node = node.parent;
        }
        return node.parent;

    }
}
```
