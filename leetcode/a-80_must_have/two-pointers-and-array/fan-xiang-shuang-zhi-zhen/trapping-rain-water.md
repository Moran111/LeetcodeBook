# Trapping Rain Water

Description

Given _n_ non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it is able to trap after raining.

![Trapping Rain Water](https://lintcode-media.oss-us-west-1.aliyuncs.com/problem/rainwatertrap.png)

Example

**Example 1:**

```
Input: [0,1,0]
Output: 0
```

**Example 2:**

```
Input: [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
```

Challenge

O(n) time and O(1) memory

O(n) time and O(n) memory is also acceptable.



```
public class Solution {
    /**
     * @param heights: a list of integers
     * @return: a integer
     */
    public int trapRainWater(int[] heights) {
        // write your code here
        int res = 0;
        int left = 0; int right = heights.length - 1;
        int maxLeft = 0; int maxRight = 0;
        while (left <= right) {
            if (heights[left] <= heights[right]) {
                maxLeft = Math.max(maxLeft, heights[left]);
                res += maxLeft - heights[left];
                left++;
            } else {
                maxRight = Math.max(maxRight, heights[right]);
                res += maxRight - heights[right];
                right--;
            }
        }
        return res;
    }
}
```
