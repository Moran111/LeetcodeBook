# Word Ladder

Description

Given two words (`start` and `end`), and a dictionary, find the shortest transformation sequence from `start` to `end`, output the length of the sequence.\
Transformation rule such that:

1. Only one letter can be changed at a time
2. Each intermediate word must exist in the dictionary. (Start and end words do not need to appear in the dictionary )

***

_Contact me on wechat to get more tips and learning materials . (wechat id : jiuzhang15)_

* Return 0 if there is no such transformation sequence.
* All words have the same length.
* All words contain only lowercase alphabetic characters.
* You may assume no duplicates in the dictionary.
* You may assume beginWord and endWord are non-empty and are not the same.
* $$len(dict)<=5000,len(start)<5len(dict)<=5000,len(start)<5$$

Example

**Example 1:**

Input:

```
start = "a"
end = "c"
dict =["a","b","c"]
```

Output:

```
2
```

Explanation:

"a"->"c"

**Example 2:**

Input:

```
start ="hit"
end = "cog"
dict =["hot","dot","dog","lot","log"]
```

Output:

```
5
```

Explanation:

"hit"->"hot"->"dot"->"dog"->"cog"

2 way bfs, check if String from forward queue exist in backward Queue or otherwise. If it exist, we can return the distance.

```
public class Solution {
    /*
     * @param start: a string
     * @param end: a string
     * @param dict: a set of string
     * @return: An integer
     */
    public int ladderLength(String start, String end, Set<String> dict) {
        // write your code here
        Queue<String> forwardQueue = new LinkedList<>();
        Set<String> fVisited = new HashSet<>();
        Queue<String> backwardQueue = new LinkedList<>();
        Set<String> bVisited = new HashSet<>();

        int level = 1;
        forwardQueue.offer(start);
        fVisited.add(start);

        backwardQueue.offer(end);
        bVisited.add(end);

        while(!forwardQueue.isEmpty() && !backwardQueue.isEmpty()) {
            level++;
            if(bfs(dict, forwardQueue, bVisited, fVisited)) {
                return level;
            }
            level++;
            if (bfs(dict, backwardQueue, fVisited, bVisited)) {
                return level;
            }
        }
        return 0;
    }

    // return if String from forward queue exist in backward Queue or otherwise
    private boolean bfs(Set<String> dict, 
                        Queue<String> queue, 
                        Set<String> oppositeVisited, 
                        Set<String> visited) {
        int size = queue.size();
        for (int q = 0; q < size; q++){
            String curr = queue.poll();
            char[] arr = curr.toCharArray();
            for (int i = 0; i < arr.length; i++) {
                char temp = arr[i];
                for (char c = 'a'; c <= 'z'; c++) {
                    if (c != arr[i]) {
                        arr[i] = c;
                        String next = String.valueOf(arr);
                        if (!dict.contains(next) || visited.contains(next)) {
                            continue;
                        }
                        if (oppositeVisited.contains(next)) {
                            return true;
                        }
                        System.out.println("b".equals(next));
                        queue.offer(next);
                        visited.add(next);
                    }
                }
                arr[i] = temp;
            }
        }
        return false;
    }
}
```
