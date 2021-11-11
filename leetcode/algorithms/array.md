# Array

## Sort&#x20;

### Quick sort  (NLogN, O(1))

#### 215. kth largest Element

#### Median (n/2) same with 215

#### 4. Median of Two Sorted Array (quick select and merge sort)

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

&#x20;

**Example 1:**

```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

**Example 2:**

```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

**Example 3:**

```
Input: nums1 = [0,0], nums2 = [0,0]
Output: 0.00000
```

**Example 4:**

```
Input: nums1 = [], nums2 = [1]
Output: 1.00000
```

**Example 5:**

```
Input: nums1 = [2], nums2 = []
Output: 2.00000
```

&#x20;

**Constraints:**

* `nums1.length == m`
* `nums2.length == n`
* `0 <= m <= 1000`
* `0 <= n <= 1000`
* `1 <= m + n <= 2000`
* `-106 <= nums1[i], nums2[i] <= 106`

1. 找整个sorted list的里的第k个数 -> 但现在我们有2个sorted list，所以我们找每个的k/2个
2. 扔掉不需要的k/2个数，这样慢慢的，当A或者B被扔完了的时候，剩下的第一个就是，或者k=1的时候，AB里面最小的就是
3. 在扔的时候我们要比较A\[k/2-1] and B\[k/2-1], if A < B, then remove A otherwise, remove B, reason is provided below
4. keep doing this until we reach the case 2

```
// Some code
2
1
3 4   find 2th, find 3th
|


find kth element in the whole sorted list
-> find k/2 element in sorted list A and find k/2 element in sorted list B
-> compare A[k/2] and B[k/2], remove the k/2 element which will not inlcude kth

1 3 9
  |
  s
2 4 5 6 
  |
  l
we want to find 4th largest element 
we will not lose k when we remove s, 
becuase s < l, 
at most, there are 3 number will be removed 
so we are safe

if (k/2 in A or in B is less than their length, then we are not going to move them)
want to find k/2 
1   3   9
2

in A, we have k/2 element
in B, we also want k/2 element, but right now, B is less than k/2, so cannot remove anything in B
even though all element in B is less than A[k/2]

// code
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length; int n = nums2.length;
        // find kth number in combine of nums1 and nums2
        int total = m + n;
        if (total % 2 == 0) {
            double left = findKth(nums1, 0, nums2, 0, total/2);
            double right = findKth(nums1, 0, nums2, 0, total/2 + 1);
            System.out.println(left + " " + right);
            return (left + right)/2.0;
        } 
        return findKth(nums1, 0, nums2, 0, total/2 + 1); 
    }
    
    // find kth element in the sorted array ascending order
    private double findKth(int[] nums1, int startOfA, int[] nums2, int startOfB, int k) {
        if (startOfA >= nums1.length) {
            // removed all element in A
            return nums2[startOfB + k - 1];
        }
        if (startOfB >= nums2.length) {
            // removed all element in B
            return nums1[startOfA + k - 1];
        }
        if (k == 1) {
            return Math.min(nums1[startOfA], nums2[startOfB]);
        }
        
        int halfKOfAIndex = startOfA + k/2 - 1;
        int halfKOfBIndex = startOfB + k/2 - 1;
        int halfOfA = Integer.MAX_VALUE;
        int halfOfB = Integer.MAX_VALUE;
        if (nums1.length >= k/2) {
            halfOfA = nums1[halfKOfAIndex];
        }
        if (nums2.length >= k/2) {
            halfOfB = nums2[halfKOfBIndex];
        }
        if (halfOfA < halfOfB) {
            return findKth(nums1, halfKOfAIndex+1, nums2, startOfB, k - k / 2);
        } else {
            return findKth(nums1, startOfA, nums2, halfKOfBIndex+1, k - k / 2);
        }
    } 
}
```

### Merge sort (NLogN, O(N))

if it is LinkedList, then we don't need O(n) space when we doing merge ...&#x20;

#### 88 Merge Sorted Array

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be _stored inside the array _`nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

**Example 1:**

```
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.
```

**Example 2:**

```
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].
```

**Example 3:**

```
Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.
```

**Constraints:**

* `nums1.length == m + n`
* `nums2.length == n`
* `0 <= m, n <= 200`
* `1 <= m + n <= 200`
* `-109 <= nums1[i], nums2[j] <= 109`

```
// Some code
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        // 谁大谁放在后面
        int idx = m + n - 1;
        int left = m - 1;
        int right = n - 1;
        while (left >= 0 && right >= 0) {
            if (nums1[left] >= nums2[right]) {
                nums1[idx] = nums1[left];
                left--;
            } else {
                nums1[idx] = nums2[right];
                right--;
            }
            idx--;
        }
        
        while (left >= 0) {
            nums1[idx--] = nums1[left--];
        }
        
        while(right >= 0) {
            nums1[idx--] = nums2[right--];
        }
    }
}
```

#### 349. Intersection of Two Arrays

Given two integer arrays `nums1` and `nums2`, return _an array of their intersection_. Each element in the result must be **unique** and you may return the result in **any order**.

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
Explanation: [4,9] is also accepted.
```

**Constraints:**

* `1 <= nums1.length, nums2.length <= 1000`
* `0 <= nums1[i], nums2[i] <= 1000`

```
// Some code
sort nums1 
sort nums2
merge sort 
```

### Heap sort (NLogN, O(1))

## Prefix Sum

### 560. Subarray Sum Equals K

Given an array of integers `nums` and an integer `k`, return _the total number of continuous subarrays whose sum equals to `k`_.

**Example 1:**

```
Input: nums = [1,1,1], k = 2
Output: 2
```

**Example 2:**

```
Input: nums = [1,2,3], k = 3
Output: 2
```

&#x20;

**Constraints:**

* `1 <= nums.length <= 2 * 104`
* `-1000 <= nums[i] <= 1000`
* `-107 <= k <= 107`

```
class Solution {
    public int subarraySum(int[] nums, int k) {
        int res = 0;
        // prefix sum
        Map<Integer, Integer> map = new HashMap<>();
        int cumulativeSum = 0;
        map.put(0, 1);
        for (int i = 0; i < nums.length; i++) {
            cumulativeSum += nums[i];
            // 找target的时候不包含它自己
            /*
            [1]
            0
            */
            int target = cumulativeSum - k;
            if (map.containsKey(target)) {
                res += map.get(target);
            }
            map.put(cumulativeSum, map.getOrDefault(cumulativeSum, 0) + 1);
        }
        return res;
    }
}
```

### 238. Product of Array Except Self



Given an integer array `nums`, return _an array_ `answer` _such that_ `answer[i]` _is equal to the product of all the elements of_ `nums` _except_ `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

&#x20;

**Example 1:**

```
Input: nums = [1,2,3,4]
Output: [24,12,8,6]
```

**Example 2:**

```
Input: nums = [-1,1,0,-3,3]
Output: [0,0,9,0,0]
```

**Constraints:**

* `2 <= nums.length <= 105`
* `-30 <= nums[i] <= 30`
* The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

```
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int[] prefixLeft = new int[nums.length+1];
        prefixLeft[0] = 1;
        for (int i = 0; i < nums.length; i++) {
            prefixLeft[i+1] = prefixLeft[i] * nums[i];
        }
        int[] prefixRight = new int[nums.length+1];
        prefixRight[nums.length] = 1;
        for (int i = nums.length-1; i >= 0; i--) {
            prefixRight[i] = prefixRight[i+1] * nums[i];
        }
        
        int[] res = new int[nums.length];
        for (int i = 1; i <= nums.length; i++) {
            res[i-1] = prefixLeft[i-1] * prefixRight[i];
        }
        return res;
    }
}

//  [1, 2,  3,  4]
// 1 1  2   6   24 
//   24 24  12  4   1
```

O(1) space?&#x20;

```
class Solution {
    public int[] productExceptSelf(int[] nums) {
        int length = nums.length;
        
        int[] answer = new int[length];
        answer[0] = 1;
        // left product
        for (int i = 1; i < length; i++) {
            answer[i] = nums[i-1] * answer[i-1];
        }
        
        int right = 1;
        for (int i = length-1; i >= 0; i--) {
            answer[i] = answer[i] * right;
            right *= nums[i];
        }
        
        return answer;
    }
}

//  [1, 2,  3,  4]
// 1 1  2   6   24 
//   24 24  12  4   1

// ans
//   1  1   2   6 
// right
//      24  12  4   1
```

## Two Pointer

### 从两边到中间

#### 680. Valid Palindrome II



Given a string `s`, return `true` _if the _`s`_ can be palindrome after deleting **at most one** character from it_.

&#x20;

**Example 1:**

```
Input: s = "aba"
Output: true
```

**Example 2:**

```
Input: s = "abca"
Output: true
Explanation: You could delete the character 'c'.
```

**Example 3:**

```
Input: s = "abc"
Output: false
```

&#x20;

**Constraints:**

* `1 <= s.length <= 105`
* `s` consists of lowercase English letters.

```
// 需要考虑每一个case，在面试的时候，写代码前要想到
// 不好描述的时候可以具一个例子，或者多举几个例子
// 这个题比较坑的case是那种abaab
//                  　  l   r
// 这个例子如果按我的逻辑，会从左边和右边里选择一个删除
// but for this question，we can only delete the l to make the rest of the string valid 
// so this means if we reverse the s and do it again
// eigher one works, then we can return true

```
