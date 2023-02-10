# 315. Count of Smaller Numbers After Self





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

## Merge Sort&#x20;

```
   int[] temp = new int[nums.length];
   
   private void mergeSort(int[] nums, int start, int end){
        if (end - start+1 <= 1) return; //Already sorted.
        int mi = start + (end - start)/ 2;
        mergeSort(nums, start, mi);
        mergeSort(nums, mi+1, end);
        merge(nums, start,mi, end);
    }
    
   private void merge(int[] nums, int start, int mi, int end){
        int lp = start;
        int rp = mi + 1;
    
        int t = start; // temp index
        
        while (lp <= mi && rp <= end){
            if (nums[lp] < nums[rp]){
                temp[t++] = nums[lp++];
            }else{
                temp[t++] = nums[rp++];
            }
        }
        
        while (lp <= mi) temp[t++] = nums[lp++];
        while (rp <= end) temp[t++] = nums[rp++];
        
        //Now copy sorted temp arr into original array
        for (int i = start; i <= end; i++){
            nums[i] = temp[i];
        }
    }
```

To apply merge sort, one key observation is that:

> The smaller elements on the right of a number will **jump from its right to its left** during the sorting process.

During merge process, we can know how many number in the right can be moved the the left. But for this question, We need to know the original index of each number. So we can put the result in the correct position after merge.&#x20;

We need some Pair \<number, index>, index will save its original index in num array. Even though the position changed after merge, we still can know its original index.&#x20;

class Solution { int\[]\[] sortedArray = null; int\[] count = null; int\[]\[] arr = null; public List countSmaller(int\[] nums) { sortedArray = new int\[nums.length]\[2]; count = new int\[nums.length];

class Solution { int\[]\[] sortedArray = null; int\[] count = null; int\[]\[] arr = null; public List countSmaller(int\[] nums) { sortedArray = new int\[nums.length]\[2]; count = new int\[nums.length];

```
 class Solution {
    int[][] sortedArray = null;
    int[] count = null;
    int[][] arr = null;
    public List<Integer> countSmaller(int[] nums) {
        sortedArray = new int[nums.length][2];
        count = new int[nums.length];
        
        arr = new int[nums.length][2];
        // [value][index]
        for(int i = 0; i < nums.length; i++) {
            arr[i][0] = nums[i];
            arr[i][1] = i;
        }
        
        mergeSort(arr, 0, nums.length-1);
        
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            result.add(count[i]);
        }
        return result;
    }
    
    // [p ... q q+1 ... r]
    private void mergeSort(int[][] arr, int start, int end) {
        if (start >= end) {
            return;
        }
        int mid = start + (end - start)/2;
        
        mergeSort(arr, start, mid);
        mergeSort(arr, mid+1, end);
         
        int i = start;
        // merge
        int rightCount = 0;
        int index1 = start;
        int index2 = mid+1;
        while (index1 <= mid && index2 <= end) {
            if (arr[index2][0] < arr[index1][0]) {
                sortedArray[i] = arr[index2];
                rightCount++;
                index2++;
            } else {
                sortedArray[i] = arr[index1];
                count[arr[index1][1]] += rightCount;
                index1++;
            }
            i++;
        }
        while(index1 <= mid) {
            sortedArray[i] = arr[index1];
            count[arr[index1][1]] += rightCount;
            index1++;
            i++;
        }
        while(index2 <= end) {
            sortedArray[i] = arr[index2];
            index2++;
            i++;
        }
        
        for (int j = start; j <= end; j++) {
            arr[j] = sortedArray[j];
        }
    }
}

```
