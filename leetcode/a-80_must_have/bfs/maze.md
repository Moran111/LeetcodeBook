# Maze

## 490. The Maze

Description

There is a ball in a maze with empty spaces and walls. The ball can go through empty spaces by rolling `up`, `down`, `left` or `right`, `but it won't stop rolling until hitting a wall`. When the ball stops, it could choose the next direction.

Given the ball's start position, the destination and the maze, determine whether the ball could stop at the destination.

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The start and destination coordinates are represented by row and column indexes.

1.There is only one ball and one destination in the maze.\
2.Both the ball and the destination exist on an empty space, and they will not be at the same position initially.\
3.The given maze does not contain border (like the red rectangle in the example pictures), but you could assume the border of the maze are all walls.\
5.The maze contains at least 2 empty spaces, and both the width and height of the maze won't exceed 100.

Example

Example 1:

```
Input:
map = 
[
 [0,0,1,0,0],
 [0,0,0,0,0],
 [0,0,0,1,0],
 [1,1,0,1,1],
 [0,0,0,0,0]
]
start = [0,4]
end = [3,2]
Output:
false
```

Example 2:

```
Input:
map = 
[[0,0,1,0,0],
 [0,0,0,0,0],
 [0,0,0,1,0],
 [1,1,0,1,1],
 [0,0,0,0,0]
]
start = [0,4]
end = [4,4]
Output:
true
```

用queue记录我们走过的点，也就是我们下一步bfs的开始的点，这种题就是可以沿着一个方向一直走，所以我们从direction map里选一个d，然后while这个方向一直走，但是while结束的时候，有可能走到boundary的外面，while结束的时候，是多走了一步的，所以我们要把这一步加回来

```
while(nr >= 0 && nc >= 0 && nr < m && nc < n && maze[nr][nc] == 0) {
                    nr += dx[d];
                    nc += dy[d];
}
// after while loop, nr may < 0 and nc < 0, reach boundary, I move one more step, 
// so need to nr - dx[d] and nc - dy[d] add it back
// this is my next row and col
if (!visited[nr - dx[d]][nc - dy[d]]) {
```

```
public class Solution {
    /**
     * @param maze: the maze
     * @param start: the start
     * @param destination: the destination
     * @return: whether the ball could stop at the destination
     */
    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        // write your code here
        int m = maze.length; int n = maze[0].length;
        Queue<int[]> queue = new LinkedList<>();
        boolean[][] visited = new boolean[m][n];

        int[] dx = new int[]{0,0,-1,1};
        int[] dy = new int[]{-1,1,0,0};

        queue.offer(start);
        visited[start[0]][start[1]] = true;

        while(!queue.isEmpty()) {
            int[] curr = queue.poll();
            if (curr[0] == destination[0] && curr[1] == destination[1]) {
                return true;
            }
            // rolling until hitting the wall
            for (int d = 0; d < 4; d++) {
                int nr = curr[0]; 
                int nc = curr[1];
                while(nr >= 0 && nc >= 0 && nr < m && nc < n && maze[nr][nc] == 0) {
                    nr += dx[d];
                    nc += dy[d];
                }
                // after while loop, this is my next row and col
                if (!visited[nr - dx[d]][nc - dy[d]]) {
                    queue.offer(new int[]{nr - dx[d], nc - dy[d]});
                    visited[nr - dx[d]][nc - dy[d]] = true;
                }
            }
        }
        return false;
    }
}
```
