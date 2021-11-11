---
description: 567. Permutation in String (similar)
---

# 438 Find All Anagrams in a String



Given two strings `s` and `p`, return _an array of all the start indices of _`p`_'s anagrams in _`s`. You may return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

**Example 1:**

```
Input: s = "cbaebabacd", p = "abc"
Output: [0,6]
Explanation:
The substring with start index = 0 is "cba", which is an anagram of "abc".
The substring with start index = 6 is "bac", which is an anagram of "abc".
```

**Example 2:**

```
Input: s = "abab", p = "ab"
Output: [0,1,2]
Explanation:
The substring with start index = 0 is "ab", which is an anagram of "ab".
The substring with start index = 1 is "ba", which is an anagram of "ab".
The substring with start index = 2 is "ab", which is an anagram of "ab".
```

**Constraints:**

* `1 <= s.length, p.length <= 3 * 104`
* `s` and `p` consist of lowercase English letters.

```
class Solution {
    public List<Integer> findAnagrams(String s, String p) {
        List<Integer> res = new ArrayList<>();
        // window size 3
        Map<Character, Integer> map = new HashMap<>(); // count frequency in p
        for (char c: p.toCharArray()) {
            map.put(c, map.getOrDefault(c, 0)+1);
        }
        
        Map<Character, Integer> window = new HashMap<>();
        int right = 0;
        for (int left = 0; left < s.length(); left++) {
            // window size <= p
            while (right < s.length() && right - left + 1 <= p.length()) {
                char rightChar = s.charAt(right);
                window.put(rightChar, window.getOrDefault(rightChar, 0)+1);
                
                // update result
                if (window.size() == map.size() && window.equals(map)) {
                    res.add(left);
                }
                
                right++;
            }
            
            char leftChar = s.charAt(left);
            window.put(leftChar, window.get(leftChar)-1);
            if (window.get(leftChar) == 0) {
                window.remove(leftChar);
            }
        }
        return res;
    }
}
```
