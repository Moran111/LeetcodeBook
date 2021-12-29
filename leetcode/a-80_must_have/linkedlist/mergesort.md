---
description: Time complexity NLogN
---

# MergeSort

Merge Sort

1. Base case: when start >= end (only one element in the list)
2. find the middle&#x20;
3. sort 0 - middle and middle + 1 - end
4. merge two sorted list

Time Complexity: NLogN

## 148. Sort List

Given the `head` of a linked list, return _the list after sorting it in **ascending order**_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/14/sort\_list\_1.jpg)

```
Input: head = [4,2,1,3]
Output: [1,2,3,4]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/14/sort\_list\_2.jpg)

```
Input: head = [-1,5,3,4,0]
Output: [-1,0,3,4,5]
```

**Example 3:**

```
Input: head = []
Output: []
```

&#x20;

**Constraints:**

* The number of nodes in the list is in the range `[0, 5 * 104]`.
* `-105 <= Node.val <= 105`

&#x20;

**Follow up:** Can you sort the linked list in `O(n logn)` time and `O(1)` memory (i.e. constant space)?

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
     * @param head: The head of linked list.
     * @return: You should return the head of the sorted linked list, using constant space complexity.
     */
    public ListNode sortList(ListNode head) {
        // merge sort takes constant space and take two sorted array
        // split the linked list to two spererate list
        if (head == null || head.next == null) {
            return head;
        }
        // cut list into two half
        ListNode prev = new ListNode(-1);
        prev.next = head;
        ListNode fast = head;
        ListNode slow = head; // mid
        while (fast != null && fast.next != null) {
            prev = prev.next;
            slow = slow.next;
            fast = fast.next.next;
        }
        prev.next = null;

        // sort each half of sorted array
        ListNode left = sortList(head);
        ListNode right = sortList(slow);
        // merge two sorted list
        return merge(left, right);
    }
    
    // merge two sorted array (need to return the sorted array)
    public ListNode merge (ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);
        ListNode tail = dummy;

        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) {
                tail.next = l1;
                tail = tail.next;
                l1 = l1.next;
            } else {
                tail.next = l2;
                tail = tail.next;
                l2 = l2.next;
            }
        }

        while (l1 != null) {
            tail.next = l1;
            tail = tail.next;
            l1 = l1.next;
        }

        while (l2 != null) {
            tail.next = l2;
            tail = tail.next;
            l2 = l2.next;
        }
        return dummy.next;
    }
}
```
