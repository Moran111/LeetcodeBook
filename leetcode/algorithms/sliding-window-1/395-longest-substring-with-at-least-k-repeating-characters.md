---
description: sliding window 变体
---

# 395 Longest Substring with At Least K Repeating Characters



Given a string `s` and an integer `k`, return _the length of the longest substring of_ `s` _such that the frequency of each character in this substring is greater than or equal to_ `k`.

**Example 1:**

```
Input: s = "aaabb", k = 3
Output: 3
Explanation: The longest substring is "aaa", as 'a' is repeated 3 times.
```

**Example 2:**

```
Input: s = "ababbc", k = 2
Output: 5
Explanation: The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.
```

**Constraints:**

* `1 <= s.length <= 104`
* `s` consists of only lowercase English letters.
* `1 <= k <= 105`

/\* sliding window: find longest substring with m different characters, in this substring each character repeated >= k times

recursion find the character whose frequency less than k. find the next character whose frequency less than k recursivly find the substring between this two character whoes frequency less than k

return: if each character in this substring occurs >= k times, then return this entire string's size \*/

```
class Solution {
    public int longestSubstring(String s, int k) {
        
        int res = 0;
        for (int i = 1; i <= 26; i++) {
            res = Math.max(res, helper(i, s, k));
        }
        return res;
    } 
    
    // 固定左边 尽力往右边挪
    private int helper(int i, String s, int k) {
        int res = 0;
        Map<Character,Integer> map = new HashMap<>(); // map  store the frequency of each character in the substring
        int count = 0; // count how many unique character which occurs >= k times
        int right = 0;
        for (int left = 0; left < s.length(); left++) {
            // move right as far as possible
            while (right < s.length() && map.size() <= i) {
                char c = s.charAt(right);
                map.put(c, map.getOrDefault(c, 0)+1);
                if (map.get(c) == k) { // must be = k, count how many unique char occur k times
                    count ++;
                }
                // update res: substring has i unique character and all unique character occurs more than k times
                if (map.size() == i && count == map.size()) {
                    res = Math.max(res, right - left + 1);
                }

                right++;
            }
            char leftChar = s.charAt(left);
            // move left, update map and count
            map.put(leftChar, map.get(leftChar)-1);
            // move left, if the char in substring less than k times, count --
            if (map.get(leftChar) == k-1) { // must be = k-1, only udpate count when its occur k-1 times
                count--;
            }
            if (map.get(leftChar) == 0) {
                map.remove(leftChar);
            }
        }
        return res;
    }
}

```
