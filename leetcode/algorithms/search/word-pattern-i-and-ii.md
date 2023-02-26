# Word Pattern I & II

Given a `pattern` and a string `s`, find if `s` follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in `pattern` and a non-empty word in `s`.

&#x20;

**Example 1:**

<pre><code><strong>Input: pattern = "abba", s = "dog cat cat dog"
</strong><strong>Output: true
</strong></code></pre>

**Example 2:**

<pre><code><strong>Input: pattern = "abba", s = "dog cat cat fish"
</strong><strong>Output: false
</strong></code></pre>

**Example 3:**

<pre><code><strong>Input: pattern = "aaaa", s = "dog cat cat dog"
</strong><strong>Output: false
</strong></code></pre>

&#x20;

**Constraints:**

* `1 <= pattern.length <= 300`
* `pattern` contains only lower-case English letters.
* `1 <= s.length <= 3000`
* `s` contains only lowercase English letters and spaces `' '`.
* `s` **does not contain** any leading or trailing spaces.
* All the words in `s` are separated by a **single space**.

Pattern should match and each pattern should not same, bijection between a letter in pattern

Two hashmap (pattern - word) (word - pattern)

```
class Solution {
    public boolean wordPattern(String pattern, String s) {
        // p - s
        Map<Character, String> map = new HashMap<>();
        //bijection between a letter in pattern
        // s - p
        Map<String, Character> map2 = new HashMap<>();
        String[] split = s.split(" ");
        if (pattern.length() != split.length) {
            return false;
        }

        for (int i = 0; i < pattern.length(); i++) {
            char c = pattern.charAt(i);
            // current pattern not equals existing pattern
            if (map.containsKey(c) && !map.get(c).equals(split[i])) {
                return false;
            } else {
                map.put(c, split[i]);
            }
            if (map2.containsKey(split[i]) && map2.get(split[i]) != c) {
                return false;
            } else {
                map2.put(split[i], c);
            }
        }
        return true;
    }
}
```

One HashMap (pattern - index, word - index)

```
/**
pattern: 'abba'
s: 'dog cat fish dog'
'a' and 'dog' -> map_index = {'a': 0, 'dog': 0}
Index of 'a' and index of 'dog' are the same.
'b' and 'cat' -> map_index = {'a': 0, 'dog': 0, 'b': 1, 'cat': 1}
Index of 'b' and index of 'cat' are the same.
'b' and 'fish' -> map_index = {'a': 0, 'dog': 0, 'b': 1, 'cat': 1, 'fish': 2}
'b' is already in the mapping, no need to update.
Index of 'b' and index of 'fish' are NOT the same. Returns False.
 */
```

```
class Solution {
    public boolean wordPattern(String pattern, String s) {
        HashMap map_index = new HashMap();
        String[] words = s.split(" ");

        if (words.length != pattern.length())
            return false;

        for (Integer i = 0; i < words.length; i++) {
            char c = pattern.charAt(i);
            String w = words[i];

            if (!map_index.containsKey(c))
                map_index.put(c, i);

            if (!map_index.containsKey(w))
                map_index.put(w, i);

            if (map_index.get(c) != map_index.get(w))
                return false;
        }

        return true;
    }
}
```
