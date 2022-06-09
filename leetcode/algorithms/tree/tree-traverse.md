# Tree Traverse

## 144. Binary Tree Preorder Traversal

Given the `root` of a binary tree, return _the preorder traversal of its nodes' values_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/15/inorder\_1.jpg)

```
Input: root = [1,null,2,3]
Output: [1,2,3]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Example 3:**

```
Input: root = [1]
Output: [1]
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[0, 100]`.
* `-100 <= Node.val <= 100`

&#x20;

**Follow up:** Recursive solution is trivial, could you do it iteratively?

preorder的顺序是root left right，stack是last in first out，我们按照top-bottom，然后right-left的顺序把这个tree放进去

```
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        // root, left, right
        
        if (root == null) {
            return new ArrayList<>();
        }
        
        List<Integer> res = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        stack.push(root);
        
        while(!stack.isEmpty()) {
            TreeNode node = stack.pop();
            res.add(node.val);
            
            // for current node
            if (node.right != null) {
                stack.push(node.right );
            }
            
            if(node.left != null) {
                stack.push(node.left);
            }
        }
        return res;
    }
    /*
          1 
       2.    3
     4      7. 8
       5 
     6
     
    1 2 4 5 6 3 7 8 
    
    
    */
```

## 94. Binary Tree Inorder Traversal



Given the `root` of a binary tree, return _the inorder traversal of its nodes' values_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/15/inorder\_1.jpg)

```
Input: root = [1,null,2,3]
Output: [1,3,2]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Example 3:**

```
Input: root = [1]
Output: [1]
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[0, 100]`.
* `-100 <= Node.val <= 100`

root-> left -> right, 先把root的所有左边都加进去，这样他们就会最后弹来，弹出来的时候检查一下这个node有没有右边，如果有右边就把当前右边和右边的所有左边放进去 -> 这样就会把所有的点都放进去

```
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        // root left right
        List<Integer> res = new ArrayList<>();
        Deque<TreeNode> stack = new ArrayDeque<>();
        
        TreeNode curr = root;
        while(curr != null) {
            stack.push(curr);
            curr = curr.left;
        }
        
        while(!stack.isEmpty()) {
            TreeNode node = stack.pop();
            res.add(node.val);
            TreeNode right = node.right;
            while (right != null) {
                stack.push(right);
                right = right.left;
            }
        }
        return res;
    }
    
    /*
        1 
       2. 3
     4   7. 8
       5 
     6
     
     first round: 1 2 4
     pop 1, add 3 7 5 6 -> 2 4 3 7 5 6
     
     second round:
     pop 2, right == null
     
     thrid round: 
     pop 4
     
     next roung 
     pop 3, add 8 -> 7 5 6 8
    */
}
```

## 145. Binary Tree Postorder Traversal

Given the `root` of a binary tree, return _the postorder traversal of its nodes' values_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/28/pre1.jpg)

```
Input: root = [1,null,2,3]
Output: [3,2,1]
```

**Example 2:**

```
Input: root = []
Output: []
```

**Example 3:**

```
Input: root = [1]
Output: [1]
```

&#x20;

**Constraints:**

* The number of the nodes in the tree is in the range `[0, 100]`.
* `-100 <= Node.val <= 100`

&#x20;

**Follow up:** Recursive solution is trivial, could you do it iteratively?

先把自己的所有左边child加进去，如果当前弹出来的有右边，那么就把右边的左边放进去，如果当前弹出来的没有右边，那么我们就直接把当前弹出来的加进去

```
/*
      1 
   2.    3
 4      7. 8
   5 
 6
 
6 5 4 2 7 8 3 1

1 2 4
round1: 4 -> 4 has right -> 5  5 has left-> 6 =>  1 2 4 5 6
round2: 6 -> 6 not has right -> leaf node  =>  1 2 4 5
round3: 5 -> 5 not has right ->            =>  1 2 4
round4: 4 -> 4 has right but 4's right just added (5) => like no right child
这个node的右边一定刚刚加进去，所以要记录刚刚加到res里的是什么

*/
```

```
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        // 先把所有的左边child加进去
        // 如果有右边，就加右边，如果没有右边，且他自己的左边也都处理了，就弹出来自己
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

## 589. N-ary Tree Preorder Traversal

Given the `root` of an n-ary tree, return _the preorder traversal of its nodes' values_.

Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value (See examples)

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

```
Input: root = [1,null,3,2,4,null,5,6]
Output: [1,3,5,6,2,4]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/11/08/sample\_4\_964.png)

```
Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: [1,2,3,6,7,11,14,4,8,12,5,9,13,10]
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[0, 104]`.
* `0 <= Node.val <= 104`
* The height of the n-ary tree is less than or equal to `1000`.

&#x20;

**Follow up:** Recursive solution is trivial, could you do it iteratively?

```
class Solution {
    public List<Integer> preorder(Node root) {
        // root, child from left to right
        
        List<Integer> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        
        Deque<Node> stack = new ArrayDeque<>();
        
        stack.push(root);
        while(!stack.isEmpty()) {
            Node curr = stack.pop();
            res.add(curr.val);
            
            List<Node> children = curr.children;
            for (int i = children.size() - 1; i >= 0; i--) {
                Node child = children.get(i);
                if (child != null) {
                    stack.push(child);
                }
            }
        }
        
        return res;
    }
}
```

## 590. N-ary Tree Postorder Traversal

Given the `root` of an n-ary tree, return _the postorder traversal of its nodes' values_.

Nary-Tree input serialization is represented in their level order traversal. Each group of children is separated by the null value (See examples)

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/12/narytreeexample.png)

```
Input: root = [1,null,3,2,4,null,5,6]
Output: [5,6,3,2,4,1]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/11/08/sample\_4\_964.png)

```
Input: root = [1,null,2,3,4,5,null,null,6,7,null,8,null,9,10,null,null,11,null,12,null,13,null,null,14]
Output: [2,6,14,11,7,3,12,8,4,13,9,10,5,1]
```

&#x20;

**Constraints:**

* The number of nodes in the tree is in the range `[0, 104]`.
* `0 <= Node.val <= 104`
* The height of the n-ary tree is less than or equal to `1000`.

&#x20;

**Follow up:** Recursive solution is trivial, could you do it iteratively?

```
class Solution {
    public List<Integer> postorder(Node root) {
        // left right root
        LinkedList<Integer> res = new LinkedList<>();
        if (root == null) {
            return res;
        }
        
        Deque<Node> stack = new ArrayDeque<>();
        stack.push(root);
        
        while(!stack.isEmpty()) {
            Node curr = stack.pop();
            res.addFirst(curr.val);
            
            List<Node> children = curr.children;
            // last in last out
            for (int i = 0; i < children.size(); i++) {
                Node child = children.get(i);
                if (child != null) {
                    stack.push(child);
                }
            }
        }
        
        return res;
    }
}
```
