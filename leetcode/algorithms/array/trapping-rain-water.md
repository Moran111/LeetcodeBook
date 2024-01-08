# Trapping Rain Water

## 42. Trapping Rain Water

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example 1:**![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

**Example 2:**

```
Input: height = [4,2,0,3,2,5]
Output: 9
```

**Constraints:**

* `n == height.length`
* `1 <= n <= 2 * 104`
* `0 <= height[i] <= 105`

```
class Solution {
    public int trap(int[] height) {
        if (height.length == 0 || height.length == 1) {
            return 0;
        }
        int maxWater = 0;
        
        int left = 0; int right = height.length-1;
        int leftMaxHeight = 0; int rightMaxHeigth = 0;
        // 就着left和right矮的那边放, 更新leftMaxheight and rightMaxHeight
        while (left < right) {
            if (height[left] < height[right]) {
                leftMaxHeight = Math.max(height[left], leftMaxHeight);
                maxWater += leftMaxHeight - height[left];
                left++;
            } else {              
                rightMaxHeigth = Math.max(height[right], rightMaxHeigth);
                maxWater += rightMaxHeigth - height[right];
                right--;
            }
        }
        return maxWater;
    }
}
```

## 407. Trapping Rain Water II&#x20;



Given an `m x n` integer matrix `heightMap` representing the height of each unit cell in a 2D elevation map, return _the volume of water it can trap after raining_.

**Example 1:**![](https://assets.leetcode.com/uploads/2021/04/08/trap1-3d.jpg)

```
Input: heightMap = [[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]]
Output: 4
Explanation: After the rain, water is trapped between the blocks.
We have two small pounds 1 and 3 units trapped.
The total volume of water trapped is 4.
```

**Example 2:**![](https://assets.leetcode.com/uploads/2021/04/08/trap2-3d.jpg)

```
Input: heightMap = [[3,3,3,3,3],[3,2,2,2,3],[3,2,1,2,3],[3,2,2,2,3],[3,3,3,3,3]]
Output: 10
```

**Constraints:**

* `m == heightMap.length`
* `n == heightMap[i].length`
* `1 <= m, n <= 200`
* `0 <= heightMap[i][j] <= 2 * 104`



Solution&#x20;

1. **What determines the amount of water can a bar can hold?** It is the min height of the max heights along all paths to the boundary (not just 4 direction!!!, which was my first intuition) Look at the example below. If we add 2 units of water into the 1 in the center, it will overflow to 0.\
   0 0 3 0 0\
   0 0 2 0 0\
   3 2 1 2 3\
   0 0 2 0 0\
   0 0 3 0 0
2. Just like 1-D two pointer approach, we need to find some boundary. Because all boundary cells cannot hold any water for sure, we use them as the initial boundary naturally.
3. Then which bar to start? Find the min bar (let's call it A) on the boundary (heap is a natural choice), then do 1 BFS (4 directions). Why BFS? Because we are sure that the amount of water that A's neighbors can hold is only determined by A now for the same reason in 1D two-pointer approach.
4. How to update the heap during BFS\
   Step 1. Remove the min bar A from the heap\
   Step 2. If A's neighbor B's height is higher, it cannot hold any water. Add it to the heap\
   Step 3. If B's height is lower, it can hold water and the amount of water should be height\_A - height\_B. **Here comes the tricky part**, we still add B's coordinate into the heap, BUT change its height to A's height because A is the max value along this path (for this reason we cannot just use heightMap and need a class/array to store it's coordinates and **UPDATED** height). And **we can think of B as a replacement of A now and never worry about A again. Therefore a new boundary is formed and we can repeat this process again**.

```
class Solution {
    // find bounardy to hold water
    public int trapRainWater(int[][] heightMap) {
        // all boundary will determine the amount of water
        // start at the smallest bar from boundary A
        // do a bfs for this bar, find how many water it can hold
        // when we do bfs, if the neigbours bar B are lower than this bar A, these diff water can be add
        // so right now the bar B is the new boundary which should has same height of A
        // otherwise, save this higher bar for later use
        
        int water = 0;
        int m = heightMap.length;
        int n = heightMap[0].length;
        
        boolean[][] visited = new boolean[m][n]; // need bfs find boundary 
        PriorityQueue<Cell> pq = new PriorityQueue<>((a, b) -> a.height - b.height); // update boundary
        for (int c = 0; c < n; c++) {
            pq.offer(new Cell(0, c, heightMap[0][c]));
            pq.offer(new Cell(m-1, c, heightMap[m-1][c]));
            visited[0][c] = true;
            visited[m-1][c] = true;
        }
        for (int r = 0; r < m; r++) {
            pq.offer(new Cell(r, 0, heightMap[r][0]));
            pq.offer(new Cell(r, n-1, heightMap[r][n-1]));
            visited[r][0] = true;
            visited[r][n-1] = true;
        }
        
        int[] dx = new int[]{0,0,-1,1};
        int[] dy = new int[]{-1,1,0,0};
        
        // start to fill water
        while (pq.size() > 0) {
            Cell lowerBoundary = pq.poll();
            // loop 4 direction of lower boundary to see how many water can get and shrink the boundary
            for (int d = 0; d < 4; d++) {
                int nx = lowerBoundary.row + dx[d];
                int ny = lowerBoundary.col + dy[d];
                if (nx < 0 || ny < 0 || nx >= m || ny >= n || visited[nx][ny]) {
                    continue;
                }
                if (heightMap[nx][ny] < lowerBoundary.height) {
                    visited[nx][ny] = true;
                    water += lowerBoundary.height - heightMap[nx][ny];
                    pq.offer(new Cell(nx, ny, lowerBoundary.height));
                } else {
                    visited[nx][ny] = true;
                    // put to pq for later use
                    pq.offer(new Cell(nx, ny, heightMap[nx][ny]));
                }
            }
        }
        
        return water;
        
    }
    
    public class Cell {
        int row;
        int col;
        int height;
        public Cell(int row, int col, int height) {
            this.row = row;
            this.col = col;
            this.height = height;
        }
    }
}


```
