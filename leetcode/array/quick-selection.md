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

## 215. Kth Largest Element in an Array

Given an integer array `nums` and an integer `k`, return _the_ `kth` _largest element in the array_.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

You must solve it in `O(n)` time complexity.

&#x20;

**Example 1:**

<pre><code><strong>Input: nums = [3,2,1,5,6,4], k = 2
</strong><strong>Output: 5
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
</strong><strong>Output: 4
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= k <= nums.length <= 105`
* `-104 <= nums[i] <= 104`

```
class Solution {
    public int findKthLargest(int[] nums, int k) {
        // quick select
        // [3,2,1,5,6,4] k = 2
        // random find a pivot, move all elements smaller than pivot to left side => how?
        // [          ] Pivot [          ]
        // smaller than P      larger than P
        // compare index pivot with N-K
        // find N - k element in which side
        // recursivly repeat prev step on that side 

    //     [3,2,1,5,6,4] k = 2, => find 6 - 2 = 4 smallest number (count from 0)
    // p:       p
    //     [1,2,3,5,6,4]
    // iP:  0 
    //        [2,3,5,6,4]
    //             p
    //     [1][2,3,4,5,6]    => find 6 - 2 - (left side array length) = find kth element

    
        return partition(nums, 0, nums.length - 1, nums.length - k);
    }

    private int partition(int[] nums, int start, int end, int k) {
        if (start >= end) {
            return nums[k];
        }

        int left = start; int right = end;
        int pivot = nums[left + (right - left)/2]; // it is the number, not the index

        while (left <= right) {
            // correct position, no need to change 
            while (left <= right && nums[left] < pivot) {
                left++;
            }
            while (left <= right && nums[right] > pivot) {
                right--;
            }
            // [3,2,1,5,6,4]
            //  l   p 
            //      r     
            // left <= pivot right >= pivot, needs to change
            if (left <= right) {
                int temp = nums[left];
                nums[left] = nums[right];
                nums[right] = temp;
                left++;
                right--;
            }
        }

        // start right left end
        if (k <= right) {
            // sort 0-right
            return partition(nums, start, right, k);
        }
        if (k >= left) {
            return partition(nums, left, end, k);
        }
        return nums[k];
    }
}
```
