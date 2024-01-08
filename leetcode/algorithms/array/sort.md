# Sort

## Merge Sort

divide array and having in merge process - having an extra space to store sorted array

\[5, 2, 6, 1] -> \[5, 2] \[6, 1] ->&#x20;

\[5] \[2] \[6] \[1]

\[2 5] \[1 6] -> \[ 1 2 5 6]

### 315 Count of Smaller Numbers After Self

You are given an integer array `nums` and you have to return a new `counts` array. The `counts` array has the property where `counts[i]` is the number of smaller elements to the right of `nums[i]`.

**Example 1:**

```
Input: nums = [5,2,6,1]
Output: [2,1,1,0]
Explanation:
To the right of 5 there are 2 smaller elements (2 and 1).
To the right of 2 there is only 1 smaller element (1).
To the right of 6 there is 1 smaller element (1).
To the right of 1 there is 0 smaller element.
```

**Example 2:**

```
Input: nums = [-1]
Output: [0]
```

**Example 3:**

```
Input: nums = [-1,-1]
Output: [0,0]
```

**Constraints:**

* `1 <= nums.length <= 105`
* `-104 <= nums[i] <= 104`

In merge process, we will move the element smaller than current one to the left of it. So we will know a number in it right smaller than it. RightCount++.  While doing the merge part, say that we are merging left\[] and right\[], left\[] and right\[] are already sorted.&#x20;

We keep a **rightcount** to record how many numbers from right\[] we have added and keep an array **count\[]** to record the result. When we move a number from right\[] into the new sorted array, we increase rightcount by 1. When we move a number from left\[] into the new sorted array, we increase count\[ index of the number ] by **rightcount**. (因为目前没有右边没有人比他小了）

![](../../../.gitbook/assets/IMG\_2163CD42DF10-1.jpeg)

count\[] array to store the current index and how many number from it right less than current number .&#x20;

We need to have a map to map the value and its index.&#x20;

```
class Solution {
    public List<Integer> countSmaller(int[] nums) {
        int[] count = new int[nums.length];
        
        // preprocessing 
        Item[] items = new Item[nums.length];
        for (int i = 0; i < nums.length; i++) {
            items[i] = new Item(nums[i], i);
        }
        
        // doing merge sort, when doing merge sort, count how many items less than
        mergeSort(items, 0, nums.length - 1, count);
        
        List<Integer> res = new ArrayList<>();
        for (int i = 0; i < count.length; i++) {
            res.add(count[i]);
        }
        
        return res;
    }
    
    private void mergeSort(Item[] items, int start, int end, int[] count) {
        // base case
        if (start >= end) {
            return;
        }
        
        int mid = start + (end - start)/2;
        mergeSort(items, start, mid, count);
        mergeSort(items, mid + 1, end, count);
        // merge
        merge(items, start, mid, mid + 1, end, count, items.length);
    }
    
    private void merge(Item[] items, int lo, int loEnd, int hi, int hiEnd, int[] count, int n) {
        Item[] sortedItems = new Item[hiEnd - lo + 1];
        int index = 0; 
        
        int leftIndex = lo; int rightIndex = hi;
        int rightCount = 0; // 右边有多少个比他小
        
        while (leftIndex <= loEnd && rightIndex <= hiEnd) {
            if (items[rightIndex].val < items[leftIndex].val) {
                rightCount++; // right count keep increase all left side is sorted, if one right less than the left, then it less than the other parts of left, rightCount will keep adding
                sortedItems[index++] = items[rightIndex++];
            } else {
                // no element less than it in its right side, so update count
                // left smaller than right
                // 比之前left小的继续加上
                count[items[leftIndex].index] += rightCount;
                sortedItems[index++] = items[leftIndex++];
            }
        }
        
        while (leftIndex <= loEnd) {
            count[items[leftIndex].index] += rightCount;
            sortedItems[index++] = items[leftIndex++];
        }
        
        while (rightIndex <= hiEnd) {
            sortedItems[index++] = items[rightIndex++];
        }
        
        // part of normal merge sort
        // copy back merged result into array
        int pos = 0;
        for (int i = lo; i <= hiEnd; i++) {
            items[i] = sortedItems[pos++];
        }
    }

    class Item {
        int val;
        int index;
        public Item(int val, int index) {
            this.val = val;
            this.index = index;
        }
    }
}
```

