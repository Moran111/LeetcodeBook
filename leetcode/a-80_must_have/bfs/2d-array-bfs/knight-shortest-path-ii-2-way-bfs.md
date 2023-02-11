# Knight Shortest Path II (2 Way BFS)

Description

Given a knight in a chessboard `n * m` (a binary matrix with 0 as empty and 1 as barrier). the knight initialze position is `(0, 0)` and he wants to reach position `(n - 1, m - 1)`, Knight can only be from left to right. Find the shortest path to the destination position, return the length of the route. Return `-1` if knight can not reached.

If the knight is at (x, y), he can get to the following positions in one step:

```
(x + 1, y + 2)
(x - 1, y + 2)
(x + 2, y + 1)
(x - 2, y + 1)
```

Example

Example 1:

```
Input:
[[0,0,0,0],[0,0,0,0],[0,0,0,0]]
Output:
3
Explanation:
[0,0]->[2,1]->[0,2]->[2,3]
```

Example 2:

```
Input:
[[0,1,0],[0,0,1],[0,0,0]]
Output:
-1
```

BFS from forward directions and also backward directions

```
class Point{
    int x,y;
    public Point(int x,int y) {
        this.x = x;
        this.y = y;
    }
}

public class Solution {
    /**
     * @param grid: a chessboard included 0 and 1
     * @return: the shortest path
     */
    int[][] FORWARD_DIRECTIONS = {
        {1, 2}, 
        {-1, 2}, 
        {2, 1},
        {-2, 1}
    };
    int[][] BACKWARD_DIRECTIONS = {
        {-1, -2},
        {1, -2}, 
        {-2, -1}, 
        {2, -1}
    };
    
    public int shortestPath2(boolean[][] grid) {
        if (grid == null || grid.length == 0) {
            return -1;
        }
        if (grid[0] == null || grid[0].length == 0) {
            return -1;
        }
        int n = grid.length;
        int m = grid[0].length;
        if (grid[n - 1][m - 1] == true) {
            return -1;
        }
        if (n * m == 1) {
            return 0;
        } 
        
        Queue<Point> forwardQueue = new LinkedList<Point>();
        Queue<Point> backwardQueue = new LinkedList<Point>();
        boolean[][] forwardSet = new boolean[n][m];
        boolean[][] backwardSet = new boolean[n][m];
        
        forwardQueue.offer(new Point(0, 0));
        backwardQueue.offer(new Point(n - 1,m - 1));
        forwardSet[0][0] = true;
        backwardSet[n - 1][m - 1] = true;
        
        int distance = 0;
        while (!forwardQueue.isEmpty() && !backwardQueue.isEmpty()) {
            distance++;
            if (extendQueue(forwardQueue, FORWARD_DIRECTIONS, forwardSet, backwardSet, grid)) {
                return distance;
            }
            
            distance++;
            if (extendQueue(backwardQueue, BACKWARD_DIRECTIONS, backwardSet, forwardSet, grid)) {
                return distance;
            }
        }
        
        return -1;
    }
    
    boolean extendQueue(Queue<Point> queue,
                        int[][] directions,
                        boolean[][] visited,
                        boolean[][] oppositeVisited,
                        boolean[][] grid) {
        int queueLength = queue.size();
        for (int i = 0; i < queueLength; i++) {
            Point head = queue.poll();
            int x = head.x;
            int y = head.y;

            for (int j = 0; j < 4; j++) {
                int newX = x + directions[j][0];
                int newY = y + directions[j][1];
                if (!isValid(newX, newY, grid, visited)) {
                    continue;
                }
                if (oppositeVisited[newX][newY] == true) {
                    return true;
                }
                
                queue.offer(new Point(newX,newY));
                visited[newX][newY] = true;
            }
        }
        
        return false;
    }

    boolean isValid(int x,
                    int y,
                    boolean[][] grid,
                    boolean[][] visited) {
        if (x < 0 || x >= grid.length) {
            return false;
        }
        if (y < 0 || y >= grid[0].length) {
            return false;
        }
        if (grid[x][y]) {
            return false;
        }
        if (visited[x][y]) {
            return false;
        }
        
        return true;
    }
}
```
