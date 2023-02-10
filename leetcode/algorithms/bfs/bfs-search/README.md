# BFS Search

## BFS**的应用一：层序遍历**

```
// 二叉树的层序遍历
void bfs(TreeNode root) {
    Queue<TreeNode> queue = new ArrayDeque<>();
    queue.add(root);
    while (!queue.isEmpty()) {
        int n = queue.size();
        for (int i = 0; i < n; i++) { 
            // 变量 i 无实际意义，只是为了循环 n 次
            TreeNode node = queue.poll();
            if (node.left != null) {
                queue.add(node.left);
            }
            if (node.right != null) {
                queue.add(node.right);
            }
        }
    }
}

作者：nettee
链接：https://leetcode-cn.com/problems/binary-tree-level-order-traversal/solution/bfs-de-shi-yong-chang-jing-zong-jie-ceng-xu-bian-l/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

层序遍历的一些变种题目：

LeetCode 103. Binary Tree Zigzag Level Order Traversal 之字形层序遍历&#x20;

LeetCode 199. Binary Tree Right Side View 找每一层的最右结点&#x20;

LeetCode 515. Find Largest Value in Each Tree Row 计算每一层的最大值&#x20;

LeetCode 637. Average of Levels in Binary Tree 计算每一层的平均值

## **BFS的应用二：最短路径 (和层有关系）**

用 BFS 的话，距离源点更近的点会先被遍历到，这样就能找到到某个点的最短路径了。

本期例题为 LeetCode「岛屿问题」系列：

* LeetCode 463. Island Perimeter 岛屿的周长（Easy）
* LeetCode 695. Max Area of Island 岛屿的最大面积（Medium）
* LeetCode 827. Making A Large Island 填海造陆（Hard）

对于最短路径问题，还有两道题目也是求网格结构中的最短路径，和我们讲解的距离岛屿的最远距离非常类似：

LeetCode 542. 01 Matrix LeetCode&#x20;

LeetCode 310. Minimum Height Trees

### 994 Rotting Oranges

You are given an `m x n` `grid` where each cell can have one of three values:

* `0` representing an empty cell,
* `1` representing a fresh orange, or
* `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return _the minimum number of minutes that must elapse until no cell has a fresh orange_. If _this is impossible, return_ `-1`.

**Example 1:**![](https://assets.leetcode.com/uploads/2019/02/16/oranges.png)

```
Input: grid = [[2,1,1],[1,1,0],[0,1,1]]
Output: 4
```

**Example 2:**

```
Input: grid = [[2,1,1],[0,1,1],[1,0,1]]
Output: -1
Explanation: The orange in the bottom left corner (row 2, column 0) is never rotten, because rotting only happens 4-directionally.
```

**Example 3:**

```
Input: grid = [[0,2]]
Output: 0
Explanation: Since there are already no fresh oranges at minute 0, the answer is just 0.
```

**Constraints:**

* `m == grid.length`
* `n == grid[i].length`
* `1 <= m, n <= 10`
* `grid[i][j]` is `0`, `1`, or `2`.

```
// 把每个rotting的橙子都放入queue，finally need to know if all orange are rotted. 
// so has a freshOrange count
class Solution {
    int[] dx = new int[]{0,0,-1,1};
    int[] dy = new int[]{-1,1,0,0};
    public int orangesRotting(int[][] grid) {
        // bfs, level
        int res = -1;
        int m = grid.length; int n = grid[0].length;
        
        int freshOrange = 0;
        Queue<int[]> queue = new LinkedList<>();
        boolean[][] visited = new boolean[m][n];
        for (int r = 0; r < m; r++) {
            for (int c = 0; c < n; c++) {
                if (grid[r][c] == 2) {
                    queue.offer(new int[]{r,c});
                    visited[r][c] = true;
                } else if (grid[r][c] == 1) {
                    freshOrange++;
                }
            }
        }
        
        int level = 0;
        while(!queue.isEmpty()) {
            int size = queue.size();
            if (freshOrange == 0) {
                return level;
            }
            for (int i = 0; i < size; i++) {
                int[] curr = queue.poll();
                for (int d = 0; d < 4; d++) {
                    int nr = curr[0] + dx[d];
                    int nc = curr[1] + dy[d];
                    if (nr < 0 || nc < 0 || nr >= m || nc >= n || visited[nr][nc]) {
                        continue;
                    }
                    if (grid[nr][nc] != 1) {
                        continue;
                    }
                    queue.offer(new int[]{nr, nc});
                    visited[nr][nc] = true;
                    freshOrange--;
                }
            }
            level++;
        }
         
        if (freshOrange == 0) {
            return 0;
        }
        return -1;
    }
}
```

## ****
