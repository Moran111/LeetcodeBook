# Fast & Slow Pointer

## Middle of the Linked List

Description

Given a non empty single linked list with head, please return the middle node of the linked list.

If there are two middle nodes, return the second middle node.

The number of nodes in the given list will be between 1 and 100.

Example

**Example 1:**

```
Input: 1->2->3->4->5->null
Output: 3->4->5->null
```

**Example 2:**

```
Input: 1->2->3->4->5->6->null
Output: 4->5->6->null
```

```
/**
 * Definition for ListNode
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */

public class Solution {
    /**
     * @param head: the head node
     * @return: the middle node
     */
    public ListNode middleNode(ListNode head) {
        // write your code here.
        ListNode slow = head;
        ListNode fast = head;
        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }
}
```

## Linked List Cycle

Description

Given a linked list, determine if it has a cycle in it.

The length of the linked list does not exceed 10000.

Example

**Example 1:**

Input:

```
linked list = 21->10->4->5，then tail connects to node index 1(value 10).
```

Output:

```
true
```

Explanation:

The linked list has rings.

**Example 2:**

Input:

```
linked list = 21->10->4->5->null
```

Output:

```
false
```

Explanation:

The linked list has no rings.

Challenge

Can you solve it without using extra space?

```
/**
 * Definition for ListNode
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */

public class Solution {
    /**
     * @param head: The first node of linked list.
     * @return: True if it has a cycle, or false
     */
    public boolean hasCycle(ListNode head) {
        // write your code here
        ListNode slow = head;
        ListNode fast = head;
        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                return true;
            }
        }
        return false;
    }
}
```
