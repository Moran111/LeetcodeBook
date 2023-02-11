# 1481. Least Number of Unique Integers after k Removals



1481\. Least Number of Unique Integers after K RemovalsMedium53450Add to ListShare

Given an array of integers `arr` and an integer `k`. Find the _least number of unique integers_ after removing **exactly** `k` elements**.**

1.

**Example 1:**

```
Input: arr = [5,5,4], k = 1
Output: 1
Explanation: Remove the single 4, only 5 is left.
```

**Example 2:**

```
Input: arr = [4,3,1,1,3,3,2], k = 3
Output: 2
Explanation: Remove 4, 2 and either one of the two 1s or three 3s. 1 and 3 will be left.
```

**Constraints:**

* `1 <= arr.length <= 10^5`
* `1 <= arr[i] <= 10^9`
* `0 <= k <= arr.length`

移除那种重复个数更加少的数字才是最佳策略

```
class Solution {
    public int findLeastNumOfUniqueInts(int[] arr, int k) {
        // sort map by value
        // Map<Integer, Integer> map = new HashMap<>();
        // for (int a: arr) {
        //     map.put(a, map.getOrDefault(a, 0)+1);
        // }
        
        // sort by value
//         PriorityQueue<int[]> pq = new PriorityQueue<>((a,b) -> a[1] - b[1]);
//         for (int key: map.keySet()) {
//             pq.add(new int[]{key, map.get(key)});
//         }
        
//         while (k > 0) {
//             int[] top = pq.peek();
//             int key = top[0];
//             // System.out.println(key);
//             map.put(key, map.get(key)-1);
//             if (map.get(key) == 0) {
//                 map.remove(key);
//                 pq.poll();
//             } 
//             k--;
//         }
        
        Map<Integer, Integer> freqMap = new HashMap<>();
        for(int x : arr){
            freqMap.put(x, freqMap.getOrDefault(x,0)+1);
        }
        //1-2, 3-3, 4-1, 2-1
        PriorityQueue<Map.Entry<Integer, Integer>> pq = 
            new PriorityQueue<>((a,b)-> a.getValue()-b.getValue());
        pq.addAll(freqMap.entrySet());
        
        while(k > 0){
            Map.Entry<Integer, Integer> entry = pq.peek();
            k = k -  entry.getValue();
            pq.poll();
        }
        
        
        return k < 0 ? pq.size()+1 : pq.size();
    }
}
```
