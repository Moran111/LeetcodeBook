# ReverseLinkedList

when reverse linked list, need a **PREV** dummy linked list, so we can do **prev** (dummy) <- node (first node want to reverse) <- node&#x20;

```
 ListNode prev = dummy;
 while(node != null) {
   ListNode next = node.next;
   node.next = prev;
   prev = node;
   node = next;
}
```

## Reverse Linked List

Reverse entire Linked List

Description

Reverse a linked list.

The linked list length is less than $$100100$$

Example

**Example 1:**

Input:

```
linked list = 1->2->3->null
```

Output:

```
3->2->1->null
```

Explanation:

Reverse Linked List

**Example 2:**

Input:

```
linked list = 1->2->3->4->null
```

Output:

```
4->3->2->1->null
```

Explanation:

Reverse Linked List

Challenge

Reverse it in-place and in one-pass

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
     * @param head: n
     * @return: The new head of reversed linked list.
     */
    public ListNode reverse(ListNode head) {
        // write your code here
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        
        ListNode node = head;
        ListNode prev = dummy;
        while(node != null) {
            ListNode next = node.next;
            node.next = prev;
            prev = node;
            node = next;
        }

        head.next = null;
        return prev;
    }

    // d 1 2 3
    //   n n1
    //      
}
```

## Reverse Linked List II

reverse m to n node in Linked List

Description

Reverse a linked list from position `m` to `n`.

m and n satisfy the following condition: $$1≤m≤n≤lengthoflist1≤m≤n≤lengthoflist.$$

Example

**Example 1:**

Input:

```
linked list = 1->2->3->4->5->NULL
m = 2
n = 4
```

Output:

```
1->4->3->2->5->NULL
```

Explanation:

Reverse the \[2,4] position of the linked list.

**Example 2:**

Input:

```
linked list = 1->2->3->4->null
m = 2
n = 3
```

Output:

```
1->3->2->4->NULL
```

Explanation:

Reverse the \[2,3] position of the linked list.

Challenge

Reverse it in-place and in one-pass

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
     * @param head: ListNode head is the head of the linked list 
     * @param m: An integer
     * @param n: An integer
     * @return: The head of the reversed ListNode
     */
    public ListNode reverseBetween(ListNode head, int m, int n) {
        // write your code here
        ListNode dummy = new ListNode(-1);
        ListNode prev = dummy;
        dummy.next = head;

        int count = 1;
        while(head != null && count < m) {
            prev = head;
            head = head.next;
            count++;
        }

        // count == m prev, head(m)
        ListNode tail = prev;
        ListNode start = head;
        System.out.println(tail.val + " " + start.val);
        for (int i = m; i <= n; i++) {
            ListNode temp = head.next;
            head.next = tail;
            tail = head;
            head = temp;
        }

        // after reverse - prev start <- xxx <- tail head (n+1)
        if (head == null) {
            // n is the last node in array, we still want to set prev.next = head
        } else {
            
        }
        prev.next = tail;
        start.next = head;
        return dummy.next;
    }
}
```

## Reverse Nodes in K-Group





