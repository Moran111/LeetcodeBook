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
