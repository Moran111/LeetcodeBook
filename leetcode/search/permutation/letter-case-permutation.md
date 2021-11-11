# Letter Case Permutation



Given a string `s`, we can transform every letter individually to be lowercase or uppercase to create another string.

Return _a list of all possible strings we could create_. You can return the output in **any order**.

**Example 1:**

```
Input: s = "a1b2"
Output: ["a1b2","a1B2","A1b2","A1B2"]
```

**Example 2:**

```
Input: s = "3z4"
Output: ["3z4","3Z4"]
```

**Example 3:**

```
Input: s = "12345"
Output: ["12345"]
```

**Example 4:**

```
Input: s = "0"
Output: ["0"]
```

**Constraints:**

* `s` will be a string with length between `1` and `12`.
* `s` will consist only of letters or digits.

In each level, has two options, add opt1 or opt2. Need to use char\[] or sb charAt(i) to change the char array itself.

```
            a1b2   i=0, when it's at a, since it's a letter, we have two branches: a, A
         /        \
       a1b2       A1b2 i=1 when it's at 1, we only have 1 branch which is itself
        |          |   
       a1b2       A1b2 i=2 when it's at b, we have two branches: b, B
       /  \        / \
      a1b2 a1B2  A1b2 A1B2 i=3  when it's at 2, we only have one branch.
       |    |     |     |
      a1b2 a1B2  A1b2  A1B2 i=4 = S.length(). End recursion, add permutation to ans. 
      
      During this process, we are changing the S char array itself
```

```
class Solution {
    public List<String> letterCasePermutation(String s) {
        List<String> res = new ArrayList<>();
        dfs(s.toCharArray(), 0, res);
        return res;
    }
    //dfs: choose or not choose, have two options in each level
    private void dfs(char[] arr, int start, List<String> res) {
        if (arr.length == start) {
            res.add(String.valueOf(arr));
            return;
        }
        
        char c = arr[start];
        if (!Character.isLetter(c)) {
            dfs(arr, start+1, res);
        } else {
            char upperCase = Character.toUpperCase(c);
            arr[start] = upperCase;
            dfs(arr, start+1, res);
            
            char lowerCase = Character.toLowerCase(c);
            arr[start] = lowerCase;
            dfs(arr, start+1, res);
        }
    }
}
```
