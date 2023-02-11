# Path Sum

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



