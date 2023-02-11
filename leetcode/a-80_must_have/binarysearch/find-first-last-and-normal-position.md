# Find First, Last, and Normal Position

## Binary Search

```
public class Solution {
    /**
     * @param nums: An integer array sorted in ascending order
     * @param target: An integer
     * @return: An integer
     */
    public int findPosition(int[] nums, int target) {
        if (nums.length == 0) {
            return -1;
        }
        int left = 0; int right = nums.length - 1;
        while(left + 1 < right) {
            int mid = left + (right - left)/2;
            if (nums[mid] > target) {
                right = mid;
            } else if (nums[mid] < target) {
                left = mid;
            } else {
                return mid;
            }
        }

        if (nums[right] == target) {
            return right;
        } else if (nums[left] == target) {
            return left;
        }
        return -1;
    }
}
```

## First Position of Target

```
public class Solution {
    /**
     * @param nums: The integer array.
     * @param target: Target to find.
     * @return: The first position of target. Position starts from 0.
     */
    public int binarySearch(int[] nums, int target) {
        // write your code here
        if (nums.length == 0) {
            return -1;
        }
        // write your code here
        int left = 0; int right = nums.length - 1;
        while(left + 1 < right) {
            int mid = left + (right - left)/2;
            if (nums[mid] >= target) {
                right = mid;
            } else {
                left = mid;
            }
        }

        if (nums[left] == target) {
            return left;
        } else if (nums[right] == target) {
            return right;
        }
        return -1;
    }
}
```

## Last Position of Target

```
public class Solution {
    /**
     * @param nums: An integer array sorted in ascending order
     * @param target: An integer
     * @return: An integer
     */
    public int lastPosition(int[] nums, int target) {
        if (nums.length == 0) {
            return -1;
        }
        // write your code here
        int left = 0; int right = nums.length - 1;
        while(left + 1 < right) {
            int mid = left + (right - left)/2;
            if (nums[mid] > target) {
                right = mid;
            } else {
                left = mid;
            }
        }

        if (nums[right] == target) {
            return right;
        } else if (nums[left] == target) {
            return left;
        }
        return -1;
    }
}
```
