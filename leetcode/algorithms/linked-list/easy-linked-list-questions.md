# Easy Linked List Questions

## 206. Reverse Linked List

Given the `head` of a singly linked list, reverse the list, and return _the reversed list_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
Input: head = [1,2]
Output: [2,1]
```

**Example 3:**

```
Input: head = []
Output: []
```

&#x20;

**Constraints:**

* The number of nodes in the list is the range `[0, 5000]`.
* `-5000 <= Node.val <= 5000`

&#x20;

**Follow up:** A linked list can be reversed either iteratively or recursively. Could you implement both?

### Solution 1: Reverse it Using Tree Pointers

It is better to have a Dummy variable (a pointer to point Null)

&#x20;                                                                                                                 \|--------|

if we don't have a Prev start with Null, then I will have **circular error**  2 <-  1 -> 2 -> 3. We also need a thrid pointer to know the next position (which is curr position)&#x20;

```
class Solution {
    public ListNode reverseList(ListNode head) {
        //          1 -> 2 -> 3 -> 4 -> 5 -> null
        //         p1    p2   n
        //          1 <- 2 -> 3 
        //         p1    p2   n
        //               p1   p2  
        // when p2 == null 
        //                         p1 <- p2    n
        //                               p1   p2
        // if (head.next == null) {
        //     return head;
        // }
        // ListNode p1 = head;
        // ListNode p2 = head.next;
        // while (p2.next != null) {
        //     ListNode next = p2.next;
        //     p2.next = p1;  // circular error p2 -> p1 -> p2 -> p3
        //     p1 = p2;
        //     p2 = next;
        // }
        // return p2;
        
        // null <- 1 <- 2 <- 3 <- 4 <- 5 <- null
        // p .     c    n
        //     <-  p    c    n
        //                        p    c    n
        //                             p    c
        ListNode prev = null;
        ListNode curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```



## 141. Linked List Cycle

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` _if there is a cycle in the linked list_. Otherwise, return `false`.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist\_test2.png)

```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.
```

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist\_test3.png)

```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

&#x20;

**Constraints:**

* The number of the nodes in the list is in the range `[0, 104]`.
* `-105 <= Node.val <= 105`
* `pos` is `-1` or a **valid index** in the linked-list.

&#x20;

**Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?

```
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;
        }
        // fast and slow pointer
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode slow = dummy;
        ListNode fast = dummy;
        
        // fast shouldn't be null
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

## 83. Remove Duplicates from Sorted List



Given the `head` of a sorted linked list, _delete all duplicates such that each element appears only once_. Return _the linked list **sorted** as well_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/list1.jpg)

```
Input: head = [1,1,2]
Output: [1,2]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/list2.jpg)

```
Input: head = [1,1,2,3,3]
Output: [1,2,3]
```

&#x20;

**Constraints:**

* The number of nodes in the list is in the range `[0, 300]`.
* `-100 <= Node.val <= 100`
* The list is guaranteed to be **sorted** in ascending order.

怎么remove duplicate 的 元素, 两个指针，a指向最后一个对的位置，b指向a后面的下一个需要比较的位置，然后a，b不一样的时候，把b加上。注意链表最后要set to null

```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        // sorted linked list
        // just have one pointer to point the last correct position
        // one pointer swap the whole list, to check if the node same with previous correct one
        ListNode curr = head;
        ListNode res = curr;
        while(head != null) {
            if (curr.val == head.val) {
                head = head.next;
            } else {
                curr.next = head;
                curr = curr.next;
            }
        }
        // 要记住最后的时候head point to null, but some duplicate node are still connect to the curr
        curr.next = null;
        return res;
    }
}

// 1 1 2
// c
//     h

// 1 1 2 3 3
//       c
//           h 


```
