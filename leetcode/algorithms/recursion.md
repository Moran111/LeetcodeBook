# Recursion

## 341 Flatten Nested List Iterator

You are given a nested list of integers `nestedList`. Each element is either an integer or a list whose elements may also be integers or other lists. Implement an iterator to flatten it.

Implement the `NestedIterator` class:

* `NestedIterator(List<NestedInteger> nestedList)` Initializes the iterator with the nested list `nestedList`.
* `int next()` Returns the next integer in the nested list.
* `boolean hasNext()` Returns `true` if there are still some integers in the nested list and `false` otherwise.

Your code will be tested with the following pseudocode:

```
initialize iterator with nestedList
res = []
while iterator.hasNext()
    append iterator.next() to the end of res
return res
```

If `res` matches the expected flattened list, then your code will be judged as correct.

&#x20;

**Example 1:**

```
Input: nestedList = [[1,1],2,[1,1]]
Output: [1,1,2,1,1]
Explanation: By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,1,2,1,1].
```

**Example 2:**

```
Input: nestedList = [1,[4,[6]]]
Output: [1,4,6]
Explanation: By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,4,6].
```

&#x20;

**Constraints:**

* `1 <= nestedList.length <= 500`
* The values of the integers in the nested list is in the range `[-106, 106]`.

```
// Some code
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return empty list if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class NestedIterator implements Iterator<Integer> {
    LinkedList<Integer> flattenList = new LinkedList<>();
    public NestedIterator(List<NestedInteger> nestedList) {
        helper(nestedList, nestedList.get(0));
    }
    
    private void helper(List<NestedInteger> nestedList, NestedInteger currInteger) {
        if (nestedList.size() == 0 && currInteger.isInteger()) {
            flattenList.add(currInteger.getInteger());
            return;
        }
        
        for (int i = 0; i < nestedList.size(); i++) {
            NestedInteger curr = nestedList.get(i);
            List<NestedInteger> list = curr.getList();
            helper(list, curr);
        }
        
    }
    
    @Override
    public Integer next() {
        if (hasNext()) {
            Integer top = flattenList.poll();
            return top;
        } 
        return null;
        
    }
    
    @Override
    public boolean hasNext() {
        if(flattenList.isEmpty()) {
            return false;
        }
        return true;
    }
}

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i = new NestedIterator(nestedList);
 * while (i.hasNext()) v[f()] = i.next();
 */
```
