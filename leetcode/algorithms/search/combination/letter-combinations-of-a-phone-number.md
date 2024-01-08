# Letter Combinations of a Phone Number

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](https://assets.leetcode.com/uploads/2022/03/15/1200px-telephone-keypad2svg.png)

&#x20;

**Example 1:**

<pre><code><strong>Input: digits = "23"
</strong><strong>Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: digits = ""
</strong><strong>Output: []
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: digits = "2"
</strong><strong>Output: ["a","b","c"]
</strong></code></pre>

&#x20;

**Constraints:**

* `0 <= digits.length <= 4`
* `digits[i]` is a digit in the range `['2', '9']`.

```
class Solution {
    public List<String> letterCombinations(String digits) {
        if (digits.length() == 0) {
            return new ArrayList<>();
        }
       Map<Character, List<Character>> map = new HashMap<>();
       map.put('2', Arrays.asList('a', 'b','c'));
       map.put('3', Arrays.asList('d', 'e','f'));
       map.put('4', Arrays.asList('g', 'h','i'));
       map.put('5', Arrays.asList('j', 'k','l'));
       map.put('6', Arrays.asList('m', 'n','o'));
       map.put('7', Arrays.asList('p', 'q','r','s'));
       map.put('8', Arrays.asList('t', 'u','v'));
       map.put('9', Arrays.asList('w','x', 'y','z'));
       
       List<String> res = new ArrayList<>();
       dfs(digits, 0, map, new ArrayList<>(), res);
       return res;
    }
    /**
     digit 8
     start 0
     
     charArr 't', 'u','v','w'
     j =      0    1
     tmp           
     res      t    u
     */

    // start point to the index of digits
    private void dfs(String digits, int start, Map<Character, List<Character>> map, 
    List<Character> tmp, List<String> res) {
        if (start == digits.length()) {
            StringBuilder sb = new StringBuilder();
            for (char t: tmp) {
                sb.append(t);
            }
            res.add(sb.toString());
            return;
        }
        // 23
        char c = digits.charAt(start);
        List<Character> charArr = map.get(c);
        //abc
        for (int j = 0; j < charArr.size(); j++) {
            tmp.add(charArr.get(j));
            // start + 1
            dfs(digits, start + 1, map, tmp, res);
            tmp.remove(tmp.size() - 1);
        }
    }
}
```
