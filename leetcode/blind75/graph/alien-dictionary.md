# Alien Dictionary

There is a foreign language which uses the latin alphabet, but the order among letters is _not_ "a", "b", "c" ... "z" as in English.

You receive a list of _non-empty_ strings `words` from the dictionary, where the words are **sorted lexicographically** based on the rules of this new language.&#x20;

Derive the order of letters in this language. If the order is invalid, return an empty string. If there are multiple valid order of letters, return **any** of them.

A string `a` is lexicographically smaller than a string `b` if either of the following is true:

* The first letter where they differ is smaller in `a` than in `b`.
* There is no index `i` such that `a[i] != b[i]` _and_ `a.length < b.length`.

**Example 1:**

```java
Input: ["z","o"]

Output: "zo"
```

Explanation:\
From "z" and "o", we know 'z' < 'o', so return "zo".

**Example 2:**

```java
Input: ["hrn","hrf","er","enn","rfnn"]

Output: "hernf"
```

Explanation:

* from "hrn" and "hrf", we know 'n' < 'f'
* from "hrf" and "er", we know 'h' < 'e'
* from "er" and "enn", we know get 'r' < 'n'
* from "enn" and "rfnn" we know 'e'<'r'
* so one possibile solution is "hernf"

**Constraints:**

* The input `words` will contain characters only from lowercase `'a'` to `'z'`.
* `1 <= words.length <= 100`
* `1 <= words[i].length <= 100`

when you create the graph:&#x20;

//words=\["wrt","wrf","er","ett","rftt","te"]\
// edge already counted\
// You’re blindly increasing the indegree, even if the edge already existed.\
// That causes an incorrect indegree count and breaks topological sorting.

if length of prev is greater than length of current, and the curr is a prefix of prev, current should located before previous. So if you see them in different order, it is not correct input.

```
class Solution {
    public String foreignDictionary(String[] words) {
      // model graph
      // h: e
      // e: r
      // n: f
      // r: n
      // Topological Sort to ensure every node appears after its predecessor
      // dependency problem
      Map<Character, Set<Character>> graph = new HashMap<>();
        Map<Character, Integer> indegree = new HashMap<>();
        for (String word: words) {
            for (char c: word.toCharArray()) {
                graph.putIfAbsent(c, new HashSet<>());
                indegree.putIfAbsent(c, 0);
            }
        }
        String prev = words[0];
        for (int i = 1; i < words.length; i++) {
            String curr = words[i];
            int minLen = Math.min(prev.length(), curr.length());
            if (prev.length() > curr.length() && prev.substring(0, minLen).equals(curr)) {
                return ""; // this part
            }
            for (int j = 0; j < minLen; j++) {
                if (prev.charAt(j) != curr.charAt(j)) {
                    //words=["wrt","wrf","er","ett","rftt","te"]
                    // edge already counted
                    // You’re blindly increasing the indegree, even if the edge already existed. 
                    // That causes an incorrect indegree count and breaks topological sorting.
                    if (!graph.get(prev.charAt(j)).contains(curr.charAt(j))) {
                        graph.get(prev.charAt(j)).add(curr.charAt(j));
                        indegree.put(curr.charAt(j), indegree.get(curr.charAt(j)) + 1);
                    }
                    break;
                }
            }
            prev = curr;
        }
        // System.out.print(graph);
        // System.out.print(indegree);

        StringBuilder sb = new StringBuilder();
        Queue<Character> queue = new LinkedList<>();
        for (char c: indegree.keySet()) {
            if (indegree.get(c) == 0) {
                queue.offer(c);
            }
        }
        while (!queue.isEmpty()) {
            char curr = queue.poll();
            sb.append(curr);
            Set<Character> set = graph.get(curr);
            for (char c: set) {
                indegree.put(c, indegree.get(c) - 1);
                if (indegree.get(c) == 0) {
                    queue.offer(c);
                }
            }
        }
        if (sb.length() == graph.size()) return sb.toString();
        return "";
    }
}

```

Time complexity:

bfs graph: O(C + E)

* Number of nodes = C
* Total edges = E (at most C² in worst case)

