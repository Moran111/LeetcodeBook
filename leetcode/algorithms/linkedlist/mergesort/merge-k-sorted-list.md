# Merge K Sorted List

Description

Merge k sorted linked lists and return it as one sorted list.

Analyze and describe its complexity.

Example

**Example 1:**

Input:

```
lists = [2->4->null,null,-1->null]
```

Output:

```
-1->2->4->null
```

Explanation:

Merge 2->4->null, nulll and -1->null into an ascending list.

**Example 2:**

Input:

```
lists = [2->6->null,5->null,7->null]
```

Output:

```
2->5->6->7->null
```

Explanation:

Merge 2->6->null, 5->null and 7->null into an ascending list.

```
/**
 * Definition for ListNode.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int val) {
 *         this.val = val;
 *         this.next = null;
 *     }
 * }
 */ 
public class Solution {
    /**
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    public ListNode mergeKLists(List<ListNode> lists) {  
        // write your code here
        ListNode dummy = new ListNode(-1);
        ListNode head = dummy;

        PriorityQueue<ListNode> pq = new PriorityQueue<>((a,b) -> a.val - b.val);
        for(ListNode node: lists) {
            if (node != null) {
                pq.offer(node);
            }
            
        }

        while(!pq.isEmpty()) {
            ListNode curr = pq.poll();
            head.next = curr;
            head = head.next;

            if (curr.next != null) {
                pq.offer(curr.next);
            }
        }
        return dummy.next;
    }
}
```
