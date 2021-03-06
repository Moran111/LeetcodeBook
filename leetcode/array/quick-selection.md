# Quick Selection

**973. K Closest Points to OriginMedium3813178Add to ListShare**

Given an array of `points` where `points[i] = [xi, yi]` represents a point on the **X-Y** plane and an integer `k`, return the `k` closest points to the origin `(0, 0)`.

The distance between two points on the **X-Y** plane is the Euclidean distance (i.e., `√(x1 - x2)2 + (y1 - y2)2`).

You may return the answer in **any order**. The answer is **guaranteed** to be **unique** (except for the order that it is in).

**Example 1:**![](https://assets.leetcode.com/uploads/2021/03/03/closestplane1.jpg)

```
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]
Explanation:
The distance between (1, 3) and the origin is sqrt(10).
The distance between (-2, 2) and the origin is sqrt(8).
Since sqrt(8) < sqrt(10), (-2, 2) is closer to the origin.
We only want the closest k = 1 points from the origin, so the answer is just [[-2,2]].
```

**Example 2:**

```
Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]
Explanation: The answer [[-2,4],[3,3]] would also be accepted.
```

```
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        // pq sort by distance
        // quick selection - pivot, in one iteration, all elements in the left of pivot is smaller than pivot
        // compare pivot and k, if pivot < k, then change pivot, need more elements
        
        int left = 0; int right = points.length-1;
        partition(points, 0, points.length-1, k);
        
        return Arrays.copyOfRange(points, 0, k);
    }
    
    private void partition(int[][] points, int start, int end, int k) {
        if (start >= end) {
            return;
        }
        int left = start; int right = end;
        // pivot is the value, not the index
        int[] pivot = points[(left + right)/2];
        
        while (left <= right) {
            // compare with pivot, left is closer to (0,0)
            while (left <= right && compare(points[left], pivot) < 0) {
                left++;
            }
            while (left <= right && compare(points[right], pivot) > 0) {
                // compared with pivot, pivot is more closer to (0,0)
                right--;
            }
            // right is more closer
            if (left <= right) {
                int[] temp = points[left];
                points[left] = points[right];
                points[right] = temp;
                left++;
                right--;
            }
        }
        
        if (left <= k) {
            partition(points, left, end, k);
        }
        if (right >= k) {
            partition(points, start, right, k);
        }
        
        // quickSort(A, start, right);
        // quickSort(A, left, end);
    }
    
    // a > b -> > 0
    // a == b == 0
    // a < b -> < 0
    private int compare(int[] a, int[] b) {
        return (a[0] * a[0] + a[1] * a[1]) - (b[0]*b[0] + b[1]*b[1]);
    }
}

/*
[[0,1],[1,0]]
2
*/
```
