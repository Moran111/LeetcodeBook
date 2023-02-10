# Permutation I & II

Permutation I

Given an array `nums` of distinct integers, return _all the possible permutations_. You can return the answer in **any order**.

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

**Constraints:**

* `1 <= nums.length <= 6`
* `-10 <= nums[i] <= 10`
* All the integers of `nums` are **unique**.

```
// has a set to store visited num
class Solution {
    List<List<Integer>> res = new ArrayList<>();
    public List<List<Integer>> permute(int[] nums) {
        dfs(nums, new ArrayList<>(), new HashSet<>());
        return res;
    }
    
    private void dfs (int[] nums, List<Integer> arr, Set<Integer> visited) {
        if (arr.size() == nums.length) {
            res.add(new ArrayList(arr));
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (visited.contains(i)) {
                continue;
            }
            visited.add(i);
            arr.add(nums[i]);
            dfs(nums, arr, visited);
            visited.remove(i);
            arr.remove(arr.size()-1);
        }
    }
}
```

Permutation II&#x20;



Given a collection of numbers, `nums`, that might contain duplicates, return _all possible unique permutations **in any order**._

**Example 1:**

```
Input: nums = [1,1,2]
Output:
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**Example 2:**

```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**Constraints:**

* `1 <= nums.length <= 8`
* `-10 <= nums[i] <= 10`

```
class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> res = new ArrayList<>();
        dfs(nums, res, new ArrayList<>(), new boolean[nums.length]);
        return res;
    }
    
    private void dfs (int[] nums, List<List<Integer>> res, List<Integer> temp, boolean[] visited) {
        if (temp.size() == nums.length) {
            res.add(new ArrayList<>(temp));
            return;
        }
        
        for (int i = 0; i < nums.length; i++) {
            if (visited[i]) {
                continue;
            }
            // if i-1 not visited, i == i-1, then skip current i
            if (i > 0 && nums[i] == nums[i-1] && !visited[i-1]) {
                continue;
            }
            temp.add(nums[i]);
            visited[i] = true;
            dfs (nums, res, temp, visited);
            visited[i] = false;
            temp.remove(temp.size()-1);
        }
    }
}
```
