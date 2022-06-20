# Graph

## 39. Combination Sum

Given an array of **distinct** integers `candidates` and a target integer `target`, return _a list of all **unique combinations** of_ `candidates` _where the chosen numbers sum to_ `target`_._ You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is **guaranteed** that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1:**

```
Input: candidates = [2,3,6,7], target = 7
Output: [[2,2,3],[7]]
Explanation:
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.
```

**Example 2:**

```
Input: candidates = [2,3,5], target = 8
Output: [[2,2,2,2],[2,3,3],[3,5]]
```

**Example 3:**

```
Input: candidates = [2], target = 1
Output: []
```

**Example 4:**

```
Input: candidates = [1], target = 1
Output: [[1]]
```

**Example 5:**

```
Input: candidates = [1], target = 2
Output: [[1,1]]
```

&#x20;

**Constraints:**

* `1 <= candidates.length <= 30`
* `1 <= candidates[i] <= 200`
* All elements of `candidates` are **distinct**.
* `1 <= target <= 500`

```
// Some code
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> result = new ArrayList<>();
        dfs(candidates, 0, target, new ArrayList<>(), result);
        return result;
    }
    
    
    // 从arr中的startIndex开始 挑选一些数，放到combination中，
    // 且他们的和为target
    private void dfs(int[] arr, int idx, int target, List<Integer> combination, List<List<Integer>> result) {
        if (target == 0) {
            result.add(new ArrayList(combination));
            return;
        }
        // [1,2] [1,3] [1,4] ... 
        for (int i = idx; i < arr.length; i++) {
            if (arr[i] > target) {
                break;
            }
            // [1] -> [1,2]
            combination.add(arr[i]);
            target -= arr[i];
            // 把所有[1,2]开头的（剩余的）和为剩余的target的集合，都找到，放在result
            dfs(arr, i, target, combination, result);
            // [1,2] -> [1] -> can go to the for loop in 18 to find solution start at [1,3]
            combination.remove(combination.size()-1);
            target += arr[i];
        }
    } 
}
```

Time complexity: related to the number of solutions&#x20;

O(答案总数 \* 构造每个答案的时间）

### 40. Combination Sum II

array will have duplicate, we need to have unique result

```
// Some code
class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> result = new ArrayList<>();
        dfs(candidates, 0, target, new ArrayList<>(), result);
        return result;
    }
    
    private void dfs(int[] arr, int idx, int target, List<Integer> combination, List<List<Integer>> result) {
        if (target == 0) {
            result.add(new ArrayList(combination));
            return;
        }
        // [1,2] [1,3] [1,4] ... 
        for (int i = idx; i < arr.length; i++) {
            if (arr[i] > target) {
                break;
            }
            // 我不是我上一个选好的那个数 (remove duplicate)
            // [1,1,7]
            // [1(1), 7] or [1(2), 7]
            // 我没有选我前面那个数，且我和我前面的那个数相等的时候，我就不能选
            if (i != idx && arr[i] == arr[i-1]) {
                continue;
            }
            // [1] -> [1,2]
            combination.add(arr[i]);
            target -= arr[i];
            // 把所有[1,2]开头的（剩余的）和为剩余的target的集合，都找到，放在result
            dfs(arr, i + 1, target, combination, result);
            // [1,2] -> [1] -> can go to the for loop in 18 to find solution start at [1,3]
            combination.remove(combination.size()-1);
            target += arr[i];
        }
    } 
}
```

## 131. Palindrome Partitioning

Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return all possible palindrome partitioning of `s`.

A **palindrome** string is a string that reads the same backward as forward.

**Example 1:**

```
Input: s = "aab"
Output: [["a","a","b"],["aa","b"]]
```

**Example 2:**

```
Input: s = "a"
Output: [["a"]]
```

**Constraints:**

* `1 <= s.length <= 16`
* `s` contains only lowercase English letters.

Solution 1

```
// Some code
class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> results = new ArrayList<>();
        dfs(s, 0, new ArrayList<>(), results);
        return results;
    }
    
    private void dfs(String s, int startIndex, 
                    List<String> partition, 
                    List<List<String>> results) {
        if (startIndex == s.length()) {
            results.add(new ArrayList(partition));
            return;
        }
        for (int i = startIndex; i < s.length(); i++) {
            // startIndex - 上一层的切割点
            // i -  这一层的切割垫
            String subString = s.substring(startIndex, i+1);
            if (isPalindrome(subString)) {
                partition.add(subString);
                dfs(s, i + 1, partition, results);
                partition.remove(partition.size() - 1);
            }
        }
    }
    
    private boolean isPalindrome(String str) {
        int left = 0; int right = str.length() - 1;
        while (left < right) {
            if (str.charAt(left) != str.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
}
```

Solution 2

```
// dfs + dp, store the isPalindrome information into a boolean[][] array, 
// because dfs will have duplicates when split
class Solution {
    boolean[][] isPalindrome = null;
    List<List<String>> results = new ArrayList<>();
    public List<List<String>> partition(String s) {
       
        getIsPalindrome(s);
        dfs(s, 0, new ArrayList<>());
        return results;
    }
    
    private void dfs(String s, int startIndex, 
                    List<Integer> partition) {
        if (startIndex == s.length()) {
            addResult(s, partition);
            return;
        }
        for (int i = startIndex; i < s.length(); i++) {
            // startIndex - 上一层的切割点
            // i -  这一层的切割垫
            if (isPalindrome[startIndex][i]) {
                partition.add(i);
                dfs(s, i + 1, partition);
                partition.remove(partition.size() - 1);
            }
        }
    }
    
    private void addResult(String s, List<Integer> parititon) {
        List<String> result = new ArrayList<>();
        int startIndex = 0;
        for (int i = 0; i < parititon.size(); i++) {
            result.add(s.substring(startIndex, parititon.get(i) + 1));
            startIndex = parititon.get(i) + 1;
        }
        results.add(result);
    }
    
    private void getIsPalindrome(String s) {
        int n = s.length();
        isPalindrome = new boolean[n][n];
        
        for (int i = 0; i < n; i++) {
            isPalindrome[i][i] = true;
        }
        
        for (int i = 0; i < n-1; i++) {
            isPalindrome[i][i+1] = (s.charAt(i) == s.charAt(i+1));
        }
        
        for (int i = n - 3; i >= 0; i--) {
            for (int j = i + 2; j < n; j++) {
                isPalindrome[i][j] = isPalindrome[i+1][j-1] && s.charAt(i) == s.charAt(j);
            }
        }
    }
}
```

### 140 World Break II



Given a string `s` and a dictionary of strings `wordDict`, add spaces in `s` to construct a sentence where each word is a valid dictionary word. Return all such possible sentences in **any order**.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

&#x20;

**Example 1:**

```
Input: s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
Output: ["cats and dog","cat sand dog"]
```

**Example 2:**

```
Input: s = "pineapplepenapple", wordDict = ["apple","pen","applepen","pine","pineapple"]
Output: ["pine apple pen apple","pineapple pen apple","pine applepen apple"]
Explanation: Note that you are allowed to reuse a dictionary word.
```

**Example 3:**

```
Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
Output: []
```

&#x20;

**Constraints:**

* `1 <= s.length <= 20`
* `1 <= wordDict.length <= 1000`
* `1 <= wordDict[i].length <= 10`
* `s` and `wordDict[i]` consist of only lowercase English letters.
* All the strings of `wordDict` are **unique**.

```
// with memorization
// Map<Integer, List<String> 
// return List<String>
class Solution {
    public List<String> wordBreak(String s, List<String> wordDict) {
        Set<String> wordSet = new HashSet(wordDict);
        // index and its combination
        Map<Integer, List<String>> memo = new HashMap<>();
        List<String> res = dfs(s, wordSet, 0, memo);
        return res;
    }
    
    // combination
    private List<String> dfs(String s, 
                     Set<String> wordSet, 
                     int startIndex,
                     Map<Integer, List<String>> memo
                    ) {
        
        if (memo.containsKey(startIndex)) {
            return memo.get(startIndex);
        }
        
        List<String> subsets = new ArrayList<>();
        if (startIndex == s.length()) {
            subsets.add("");
        }
        
        for (int i = startIndex; i < s.length(); i++) {
            String firstPart = s.substring(startIndex, i+1);
            if (wordSet.contains(firstPart)) {
                List<String> subResult = dfs(s, wordSet, i+1, memo);
                for (String str: subResult) {
                    if (str.isEmpty()) {
                        subsets.add(firstPart);
                    } else {
                        subsets.add(firstPart + " " + str);
                    }
                }
            }
        }
        
        memo.put(startIndex, subsets);
        return subsets;
    }
}
```

## 46. Permutations



Given an array `nums` of distinct integers, return _all the possible permutations_. You can return the answer in **any order**.

&#x20;

**Example 1:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**Example 2:**

```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```

**Example 3:**

```
Input: nums = [1]
Output: [[1]]
```

&#x20;

**Constraints:**

* `1 <= nums.length <= 6`
* `-10 <= nums[i] <= 10`
* All the integers of `nums` are **unique**.

```
// Some code
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        dfs(nums, new boolean[nums.length], new ArrayList<>(), res);
        return res;
    }
    
    // compared to combination, no need to pass startIndex
    private void dfs(int[] nums, boolean[] visited, 
    List<Integer> list, 
    List<List<Integer>> results) {
        
        if (list.size() == nums.length) {
            results.add(new ArrayList(list));
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (visited[i]) {
                continue;
            }
            visited[i] = true;
            list.add(nums[i]);
            dfs(nums, visited, list, results);
            visited[i] = false;
            list.remove(list.size() - 1);
        }
    }
}
```

## Permutation need to know the index

### 526. Beautiful Arrangement

Suppose you have `n` integers labeled `1` through `n`. A permutation of those `n` integers `perm` (**1-indexed**) is considered a **beautiful arrangement** if for every `i` (`1 <= i <= n`), **either** of the following is true:

* `perm[i]` is divisible by `i`.
* `i` is divisible by `perm[i]`.

Given an integer `n`, return _the **number** of the **beautiful arrangements** that you can construct_.

&#x20;

**Example 1:**

```
Input: n = 2
Output: 2
Explanation: 
The first beautiful arrangement is [1,2]:
    - perm[1] = 1 is divisible by i = 1
    - perm[2] = 2 is divisible by i = 2
The second beautiful arrangement is [2,1]:
    - perm[1] = 2 is divisible by i = 1
    - i = 2 is divisible by perm[2] = 1
```

**Example 2:**

```
Input: n = 1
Output: 1
```

**Constraints:**

* `1 <= n <= 15`

```
// Some code
class Solution {
    int count = 0;
    public int countArrangement(int n) {
        dfs(n, new ArrayList<>(), 1, new boolean[n+1]);
        return count;
    }
    
    // permutation, need to record the index of the current position
    private void dfs(int n, List<Integer> subsets, int index, boolean[] visited) {
        if (index > n) {
            for (int j = 0; j < subsets.size(); j++) {
                System.out.print(subsets.get(j) + " ");
            }
            System.out.println();
            count++;
            return;
        }
        
        for (int i = 1; i <= n; i++) {
            if (visited[i]) {
                continue;
            }
            if (index % i == 0 || i % index == 0) {
                subsets.add(i);
                visited[i] = true;
                // should be index+1, instead of i+1
                dfs(n, subsets, index+1, visited);
                subsets.remove(subsets.size()-1);
                visited[i] = false;
            }
        }
    }
}
```

## Graph DFS

### 332. Reconstruct Itinerary



You are given a list of airline `tickets` where `tickets[i] = [fromi, toi]` represent the departure and the arrival airports of one flight. Reconstruct the itinerary in order and return it.

All of the tickets belong to a man who departs from `"JFK"`, thus, the itinerary must begin with `"JFK"`. If there are multiple valid itineraries, you should return the itinerary that has the smallest lexical order when read as a single string.

* For example, the itinerary `["JFK", "LGA"]` has a smaller lexical order than `["JFK", "LGB"]`.

You may assume all tickets form at least one valid itinerary. You must use all the tickets once and only once.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/itinerary1-graph.jpg)

```
Input: tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
Output: ["JFK","MUC","LHR","SFO","SJC"]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/14/itinerary2-graph.jpg)

```
Input: tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
Explanation: Another possible reconstruction is ["JFK","SFO","ATL","JFK","ATL","SFO"] but it is larger in lexical order.
```

&#x20;

**Constraints:**

* `1 <= tickets.length <= 300`
* `tickets[i].length == 2`
* `fromi.length == 3`
* `toi.length == 3`
* `fromi` and `toi` consist of uppercase English letters.
* `fromi != toi`

```
// Some code
class Solution {
    int numTicket = 0;
    public List<String> findItinerary(List<List<String>> tickets) {
        Map<String, List<String>> graph = new HashMap<>();
        Map<String, boolean[]> visited = new HashMap<>();
        for (List<String> ticket : tickets) {
            graph.putIfAbsent(ticket.get(0), new ArrayList<>());
            graph.get(ticket.get(0)).add(ticket.get(1)); 
        }
        for (String key: graph.keySet()) {
            Collections.sort(graph.get(key));
            int size = graph.get(key).size();
            visited.putIfAbsent(key, new boolean[size]);
        }
        // System.out.print(graph);
        numTicket = tickets.size();
        List<String> path = new ArrayList<>(Arrays.asList("JFK"));
        List<String> result = new ArrayList<>();
        dfs(graph, "JFK", visited, path, result, 0);
        return result;
    }

    private boolean dfs(Map<String, List<String>> graph, 
                     String start, 
                     Map<String, boolean[]> visited,
                     List<String> path,
                     List<String> result,
                     int usedTicket) {
        if (usedTicket == numTicket) {
            result.addAll(path);
            return true;
        }
        if (!graph.containsKey(start)) {
            return false;
        }
    
        List<String> nexts = graph.get(start);
        boolean[] hasVisited = visited.get(start);
        for (int i = 0; i < nexts.size(); i++) {
            String next = nexts.get(i);
            if (hasVisited[i]) {
                continue;
            }
            hasVisited[i] = true;
            path.add(next);
            boolean done = dfs(graph, next, visited, path, result, usedTicket + 1);
            if (done) {
                return true;
            }
            hasVisited[i] = false;
            path.remove(path.size()-1);
        }
        return false;
    }
}
```
