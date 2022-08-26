# Stack Part2

## 1190. Reverse Substrings Between Each Pair of Parentheses



You are given a string `s` that consists of lower case English letters and brackets.

Reverse the strings in each pair of matching parentheses, starting from the innermost one.

Your result should **not** contain any brackets.

&#x20;

**Example 1:**

<pre><code>Input: s = "(abcd)"
<strong>Output:
</strong> "dcba"</code></pre>

**Example 2:**

<pre><code>Input: s = "(u(love)i)"
<strong>Output:
</strong> "iloveu"
<strong>Explanation:
</strong> The substring "love" is reversed first, then the whole string is reversed.</code></pre>

**Example 3:**

<pre><code>Input: s = "(ed(et(oc))el)"
<strong>Output:
</strong> "leetcode"
<strong>Explanation:
</strong> First, we reverse the substring "oc", then "etco", and finally, the whole string.</code></pre>

&#x20;

**Constraints:**

* `1 <= s.length <= 2000`
* `s` only contains lower case English characters and parentheses.
* It is guaranteed that all parentheses are balanced.

正常的stack的题，括号的题，但是这个题a(b(cd)e)，最里面的（）可能被反转很多次。 所以每次取出来的时候都可以反着放回去。 也可以用recursion来做

```
class Solution {
    public String reverseParentheses(String s) {
        // stack, char put in, ( put in, ) pop until meet (, reverse the string between()
        // and put reversed string back to stack
        // if stack elements contains more chars, reverse them
        Deque<Character> stack = new ArrayDeque<>();
        for (char c: s.toCharArray()) {
            if (c == ')') {
                List<Character> temp = new ArrayList<>();
                while (!stack.isEmpty() && !stack.peekFirst().equals('(')) {
                    //  正着pull出来再反着push进去
                    temp.add(stack.removeFirst());
                }
                // top = '(' or stack is empty
                if (!stack.isEmpty()) {
                    stack.removeFirst();
                }
                // (abcd) => dcba
                for (char ch: temp) {
                    stack.addFirst(ch);
                }
            } else {
                stack.addFirst(c); 
            } 
        }
     
        StringBuilder res = new StringBuilder();
        while (stack.size() > 0) {
            res.append(stack.removeFirst());
        }
        return res.reverse().toString();
    }
    
    //"(u(love)i)" 
    // ( u (love -> (u e v o l i
    // res.toString() -> i l o v e u 
    
    // "(ed(et(oc))el)"
    // ed et oc ->  ed et co -> ed octe el
    // le etco ed
    
}
```
