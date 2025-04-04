# 139. Word Break

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

&#x20;

**Example 1:**

<pre><code><strong>Input: s = "leetcode", wordDict = ["leet","code"]
</strong><strong>Output: true
</strong><strong>Explanation: Return true because "leetcode" can be segmented as "leet code".
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: s = "applepenapple", wordDict = ["apple","pen"]
</strong><strong>Output: true
</strong><strong>Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
</strong>Note that you are allowed to reuse a dictionary word.
</code></pre>

**Example 3:**

<pre><code><strong>Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
</strong><strong>Output: false
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= s.length <= 300`
* `1 <= wordDict.length <= 1000`
* `1 <= wordDict[i].length <= 20`
* `s` and `wordDict[i]` consist of only lowercase English letters.
* All the strings of `wordDict` are **unique**.

```
    //leetcode
    //l   eetcode
    //le  etcode
    //lee tcode
    //leet code
    //     code -> start c -> c, co, code, code
```

But if we have case like:&#x20;

```
// "aaaaaaaaaaaaaab"
//  a        aaaaaaaaaaaaab => a  aaaaaaaaaaaab
//  aa       aaaaaaaaaaaab
```

The position i can be find multiple times. aaaaaaaaaaaab is found multiple times. SO after we firstly find it, we know it is not in dict, we should save it use a cache to cache if start - end exist in map.  Map\<start index, boolean>. We usually add memorization map before we RETURN the result.&#x20;

```
class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> dict = new HashSet<>(wordDict);
        // cache if start - end exist in memo
        Map<Integer, Boolean> memo = new HashMap<>();
        return dfs(s, 0, dict, memo);
    }

    private boolean dfs(String s, int start, Set<String> dict, 
    Map<Integer, Boolean> memo) {
        if (start == s.length()) {
            return true;
        }
        if (memo.containsKey(start)) {
            return memo.get(start);
        }
        for (int i = start; i < s.length(); i++) {
            // [start, i]
            String curr = s.substring(start, i+1);
            if (dict.contains(curr)) {
                //memo.put(curr, true); // means start - i+1 exist
                // find i+1 - end
                boolean res = dfs(s, i+1, dict, memo);
                if (res) {
                    memo.put(start, true);
                    return true;
                }
                // if i+1 - end is not in the dict?
                // return to prev level
            }
        }
        memo.put(start, false);
        return false;
    }
```
