# Binary Search

## 一半一半

rotated sorted array: need to make sure the relationship between two parts

![](../../.gitbook/assets/IMG\_AFAC1A640E17-1.jpeg)

### 33. Search in Rotated Sorted Array

There is an integer array `nums` sorted in ascending order (with **distinct** values).

Prior to being passed to your function, `nums` is **possibly rotated** at an unknown pivot index `k` (`1 <= k < nums.length`) such that the resulting array is `[nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]` (**0-indexed**). For example, `[0,1,2,4,5,6,7]` might be rotated at pivot index `3` and become `[4,5,6,7,0,1,2]`.

Given the array `nums` **after** the possible rotation and an integer `target`, return _the index of_ `target` _if it is in_ `nums`_, or_ `-1` _if it is not in_ `nums`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

**Example 3:**

```
Input: nums = [1], target = 0
Output: -1
```

**Constraints:**

* `1 <= nums.length <= 5000`
* `-104 <= nums[i] <= 104`
* All values of `nums` are **unique**.
* `nums` is an ascending array that is possibly rotated.
* `-104 <= target <= 104`

一半一半，target在左边一半还是右边一半，要知道mid切在左边还是mid切在右边

```
class Solution {
    public int search(int[] nums, int target) {
        int left = 0; int right = nums.length-1;
        
        while (left + 1 < right) {
            int mid = left + (right - left)/2;
            if (target == nums[mid]) {
                return mid;
            } else if (nums[mid] >= nums[left]){ // mid 切在左边
                if (nums[left] <= target && target < nums[mid]) {
                    right = mid;
                } else {
                    left = mid;
                }
            } else { // // mid 切在右边
                if (nums[mid] < target && target <= nums[right]) {
                    left = mid;
                } else {
                    right = mid;
                }
            }
              
        }
        
        if (nums[left] == target) {
            return left;
        }
        if (nums[right] == target) {
            return right;
        }
        return -1;
    }
}
```

### 153. Find Minimum in Rotated Sorted Array

Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. For example, the array `nums = [0,1,2,4,5,6,7]` might become:

* `[4,5,6,7,0,1,2]` if it was rotated `4` times.
* `[0,1,2,4,5,6,7]` if it was rotated `7` times.

Notice that **rotating** an array `[a[0], a[1], a[2], ..., a[n-1]]` 1 time results in the array `[a[n-1], a[0], a[1], a[2], ..., a[n-2]]`.

Given the sorted rotated array `nums` of **unique** elements, return _the minimum element of this array_.

You must write an algorithm that runs in `O(log n) time.`

**Example 1:**

```
Input: nums = [3,4,5,1,2]
Output: 1
Explanation: The original array was [1,2,3,4,5] rotated 3 times.
```

**Example 2:**

```
Input: nums = [4,5,6,7,0,1,2]
Output: 0
Explanation: The original array was [0,1,2,4,5,6,7] and it was rotated 4 times.
```

**Example 3:**

```
Input: nums = [11,13,15,17]
Output: 11
Explanation: The original array was [11,13,15,17] and it was rotated 4 times. 
```

**Constraints:**

* `n == nums.length`
* `1 <= n <= 5000`
* `-5000 <= nums[i] <= 5000`
* All the integers of `nums` are **unique**.
* `nums` is sorted and rotated between `1` and `n` times.

```
class Solution {
    public int findMin(int[] nums) {
        int left = 0; int right = nums.length-1;
        while (left + 1 < right) {
            int mid = left + (right - left)/2;
            if (nums[left] > nums[right]) {
                if (nums[mid] > nums[left]) {
                    left = mid;
                } else {
                    right = mid;
                }
            } else {
                if (nums[mid] > nums[left]) {
                    right = mid;
                } else {
                    left = mid;
                }
            }
        }
        
        if (nums[left] < nums[right]) {
            return nums[left];
        }
        return nums[right];
    }
}
```

### 162. Find Peak Element

A peak element is an element that is strictly greater than its neighbors.

Given an integer array `nums`, find a peak element, and return its index. If the array contains multiple peaks, return the index to **any of the peaks**.

You may imagine that `nums[-1] = nums[n] = -∞`.

You must write an algorithm that runs in `O(log n)` time.

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```

**Example 2:**

```
Input: nums = [1,2,1,3,5,6,4]
Output: 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
```

**Constraints:**

* `1 <= nums.length <= 1000`
* `-231 <= nums[i] <= 231 - 1`
* `nums[i] != nums[i + 1]` for all valid `i`.

```
class Solution {
    public int findPeakElement(int[] nums) {
        int left = 0; int right = nums.length-1;
        while (left + 1 < right) {
            int mid = left + (right - left)/2;
            
            if (nums[mid-1] < nums[mid] && nums[mid] < nums[mid+1]) {
                left = mid;
            } else if (nums[mid-1] > nums[mid] && nums[mid] > nums[mid+1]) {
                right = mid;
            } else if (nums[mid] > nums[mid-1] && nums[mid] > nums[mid+1]) {
                return mid;
            } else {
                right = mid;
            }
        }
        
        if (nums[left] > nums[right]) {
            return left;
        }
        return right;
    }
}

// mid peak, lowest point, /, \
```

### 74. Search a 2D Matrix

Write an efficient algorithm that searches for a value in an `m x n` matrix. This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

**Example 1:**![](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true
```

**Example 2:**![](https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg)

```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
Output: false
```

**Constraints:**

* `m == matrix.length`
* `n == matrix[i].length`
* `1 <= m, n <= 100`
* `-104 <= matrix[i][j], target <= 104`

```
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int r1 = 0; 
        int r2 = matrix.length-1;
        
        while (r1 <= r2) {
            int midRow = r1 + (r2 - r1)/2; 
            
            if (target < matrix[midRow][0]) {
                r2 = midRow - 1;
            } else if (target > matrix[midRow][matrix[0].length-1]) {
                r1 = midRow + 1;
            } else {
                return searchCol(matrix[midRow], target);
            }
        }
        
        return false;
    }
    
    private boolean searchCol(int[] rows, int target) {
        int left = 0; int right = rows.length-1;
        while(left + 1 < right) {
            int mid = left + (right - left)/2;
            if (rows[mid] == target) {
                return true;
            } else if (rows[mid] > target) {
                right = mid;
            } else {
                left = mid;
            }
        }
        
        if (rows[left] == target) {
            return true;
        }
        if (rows[right] == target) {
            return true;
        }
        return false;
    }
}

```

Also we can treat this 2d matrix as one sorted Array - find any position in Sorted Array

lets say you have a matrix M with 4 rows and 3 columns. When we want to access M\[2]\[1], the way the memory address is calculated is 2(i)\*3(col)+1(j) = 7.

```
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
         // convert 2d into 1d;
        // i, j -> (i * col + j) th
        
        // matrix[mid / col][mid % colmn] to convert back
        int row = matrix.length, col = matrix[0].length;
        int left = 0, right = row * col - 1;
        
        while (left + 1 < right) {
            int mid = (right - left) / 2 + left;
            int cur = matrix[mid / col][mid % col];
            if (cur == target) {
                return true;
            }
            else if (cur > target) {
                right = mid;
            }
            else {
                left = mid;
            }
        }
        
        return matrix[left / col][left % col] == target 
            || matrix[right / col][right% col] == target;
    }
}
```

## Find First/Last/Any target position in Sorted Array

### 981. Time Based Key-Value Store

Design a time-based key-value data structure that can store multiple values for the same key at different time stamps and retrieve the key's value at a certain timestamp.

Implement the `TimeMap` class:

* `TimeMap()` Initializes the object of the data structure.
* `void set(String key, String value, int timestamp)` Stores the key `key` with the value `value` at the given time `timestamp`.
* `String get(String key, int timestamp)` Returns a value such that `set` was called previously, with `timestamp_prev <= timestamp`. If there are multiple such values, it returns the value associated with the largest `timestamp_prev`. If there are no values, it returns `""`.

**Example 1:**

```
Input
["TimeMap", "set", "get", "get", "set", "get", "get"]
[[], ["foo", "bar", 1], ["foo", 1], ["foo", 3], ["foo", "bar2", 4], ["foo", 4], ["foo", 5]]
Output
[null, null, "bar", "bar", null, "bar2", "bar2"]

Explanation
TimeMap timeMap = new TimeMap();
timeMap.set("foo", "bar", 1);  // store the key "foo" and value "bar" along with timestamp = 1.
timeMap.get("foo", 1);         // return "bar"
timeMap.get("foo", 3);         // return "bar", since there is no value corresponding to foo at timestamp 3 and timestamp 2, then the only value is at timestamp 1 is "bar".
timeMap.set("foo", "bar2", 4); // store the key "foo" and value "ba2r" along with timestamp = 4.
timeMap.get("foo", 4);         // return "bar2"
timeMap.get("foo", 5);         // return "bar2"
```

**Constraints:**

* `1 <= key.length, value.length <= 100`
* `key` and `value` consist of lowercase English letters and digits.
* `1 <= timestamp <= 107`
* All the timestamps `timestamp` of `set` are strictly increasing.
* At most `2 * 105` calls will be made to `set` and `get`.

```
class TimeMap {
    // Map<key, timestamp list> 
    // timestamp is sorted by netureal 
    Map<String, List<Node>> map;

    public TimeMap() {
        map = new HashMap<>();
    }
    
    public void set(String key, String value, int timestamp) {
        map.putIfAbsent(key, new ArrayList<>());
        map.get(key).add(new Node(timestamp, value));
    }
    
    public String get(String key, int timestamp) {
        if (!map.containsKey(key)) {
            return "";
        }
        // retutn the last position for prev timestamp <= curr timestamp
        List<Node> list = map.get(key);
        int left = 0; int right = list.size()-1;
        while (left + 1 < right) {
            int mid = left + (right - left)/2;
            if (list.get(mid).timestamp > timestamp) {
                right = mid;
            } else {
                left = mid;
            }
        }
        
        if (list.get(right).timestamp <= timestamp) {
            return list.get(right).value;
        }
        if (list.get(left).timestamp <= timestamp) {
            return list.get(left).value;
        }
        return "";
    }
    
    class Node {
        int timestamp;
        String value;
        public Node(int timestamp, String value) {
            this.timestamp = timestamp;
            this.value = value;
        }
    }
}

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap obj = new TimeMap();
 * obj.set(key,value,timestamp);
 * String param_2 = obj.get(key,timestamp);
 */
```

## 二分答案

### 1011. Capacity To Ship Packages Within D Days

A conveyor belt has packages that must be shipped from one port to another within `days` days.

The `ith` package on the conveyor belt has a weight of `weights[i]`. Each day, we load the ship with packages on the conveyor belt (in the order given by `weights`). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within `days` days.

**Example 1:**

```
Input: weights = [1,2,3,4,5,6,7,8,9,10], days = 5
Output: 15
Explanation: A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
1st day: 1, 2, 3, 4, 5
2nd day: 6, 7
3rd day: 8
4th day: 9
5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed.
```

**Example 2:**

```
Input: weights = [3,2,2,4,1,4], days = 3
Output: 6
Explanation: A ship capacity of 6 is the minimum to ship all the packages in 3 days like this:
1st day: 3, 2
2nd day: 2, 4
3rd day: 1, 4
```

**Example 3:**

```
Input: weights = [1,2,3,1,1], days = 4
Output: 3
Explanation:
1st day: 1
2nd day: 2
3rd day: 3
4th day: 1, 1
```

**Constraints:**

* `1 <= days <= weights.length <= 5 * 104`
* `1 <= weights[i] <= 500`

```
class Solution {
    public int shipWithinDays(int[] weights, int days) {
        // search solution
        // min ship capacity - min of weights
        // max ship capacity - max of weights 
        // search if the least capacity to make all weights can ship in days
        int sum = 0;
        for (int i = 0; i < weights.length; i++) {
            sum += weights[i];
        }
        int left = 1; int right = sum;
        
        while (left + 1 < right) {
            int mid = left + (right - left)/2;
            if (canShipByGivenWeight(weights, days, mid)) {
                right = mid;
            } else {
                left = mid;
            }
        }
        
        if (canShipByGivenWeight(weights, days, left)) {
            return left;
        }
        return right;
    }
    
    private boolean canShipByGivenWeight(int[] weights, int days, int weightCapacity) {
        
        int i = 0;
        int j = 0;
        for (i = 0; i < days; i++) {
            if (j >= weights.length) {
                return true;
            }
            int sum = 0;
            while (j < weights.length && sum + weights[j] <= weightCapacity) {
                sum += weights[j];
                j++;
            }
        }
        // if left weight, return false
        return j >= weights.length;
    }
}

// d1: 1 2 3 4
// d2: 5 6
// d3: 7
// d4: 8
// d5: 9
```

### 287. Find the Duplicate Number



Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return _this repeated number_.

You must solve the problem **without** modifying the array `nums` and uses only constant extra space.

**Example 1:**

```
Input: nums = [1,3,4,2,2]
Output: 2
```

**Example 2:**

```
Input: nums = [3,1,3,4,2]
Output: 3
```

**Example 3:**

```
Input: nums = [1,1]
Output: 1
```

**Example 4:**

```
Input: nums = [1,1,2]
Output: 1
```

**Constraints:**

* `1 <= n <= 105`
* `nums.length == n + 1`
* `1 <= nums[i] <= n`
* All the integers in `nums` appear only **once** except for **precisely one integer** which appears **two or more** times.

**Follow up:**

* How can we prove that at least one duplicate number must exist in `nums`?
* Can you solve the problem in linear runtime complexity?

```
class Solution {
    public int findDuplicate(int[] nums) {
        
        int left = Integer.MAX_VALUE; int right = 0;
        for (int i = 0; i < nums.length; i++) {
            left = Math.min(left, nums[i]);
            right = Math.max(right, nums[i]);
        }
        
        while (left + 1 < right) {
            int mid = left + (right - left)/2;
            if (lessOrEqualTo(nums, mid) <= mid) {
                // duplicate must in the right part
                left = mid;
            } else {
                right = mid;
            }
        }
        
        if (lessOrEqualTo(nums, left) <= left) {
            return right;
        }
        return left;
    }
    
    private int lessOrEqualTo(int[] nums, int mid) {
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] <= mid) {
                count++;
            }
        }
        return count;
    }
}
```

