# Medium Linked List Question

## 92. Reverse Linked List II (Reverse inside the list)

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return _the reversed list_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

```
Input: head = [1,2,3,4,5], left = 2, right = 4
Output: [1,4,3,2,5]
```

**Example 2:**

```
Input: head = [5], left = 1, right = 1
Output: [5]
```

&#x20;

**Constraints:**

* The number of nodes in the list is `n`.
* `1 <= n <= 500`
* `-500 <= Node.val <= 500`
* `1 <= left <= right <= n`

**Follow up:** Could you do it in one pass?

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
    public ListNode reverseBetween(ListNode head, int left, int right) {
        if (head == null || head.next == null) {
            return head;
        }
        // prev left right next
        // prev start end  next
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        
        ListNode curr = dummy;
        int count = 0;
        // 1 2 3 
        while (count < left - 1) {
            curr = curr.next;
            count++;
        }
        ListNode prev = curr;
        ListNode start = prev.next;
        ListNode end = start; 
        
        while (count != right) { // when count == 3, head point to 4
            curr = curr.next;
            count++;
        }
        end = curr;
        ListNode next = end.next;
        
        reverse(start, next); // need to put right + 1 here
                
        prev.next = end; 
        start.next = next;
        return dummy.next;
    }
    
    // reverse [left, right]
    private void reverse(ListNode start, ListNode end) {
        if (start == end) {
            return;
        }
        ListNode prev = null;
        ListNode curr = start;
        while(curr != end) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
    }
}
```

更简洁也更好的写法：&#x20;

```
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        // prev start end then
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        
        ListNode prev = dummy;
        for (int i = 0; i < left - 1; i++) {
            prev = prev.next;
        }
        // p
        // 1 2 3 4 5
        //.      s t
        //   2 3 4
        ListNode start = prev.next; // prev 
        ListNode then = start.next; // curr
        for (int i = left; i < right; i++) {
            ListNode next = then.next;
            then.next = start;
            start = then;
            then = next;
        }
        
        ListNode temp = prev.next;
        prev.next = start;
        temp.next = then;
        return dummy.next;
    }
}
```

## 143. Reorder List

reverse second half of linked list and merge two list in place



You are given the head of a singly linked-list. The list can be represented as:

```
L0 → L1 → … → Ln - 1 → Ln
```

_Reorder the list to be on the following form:_

```
L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
```

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/04/reorder1linked-list.jpg)

```
Input: head = [1,2,3,4]
Output: [1,4,2,3]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/09/reorder2-linked-list.jpg)

```
Input: head = [1,2,3,4,5]
Output: [1,5,2,4,3]
```

&#x20;

**Constraints:**

* The number of nodes in the list is in the range `[1, 5 * 104]`.
* `1 <= Node.val <= 1000`

```
class Solution {
    public void reorderList(ListNode head) {
        if (head == null || head.next == null) {
            return;
        }
        // reverse second half of linked list and merge two list
        // find second half of the linked list - slow and fast, 找比较偏左的那个比较好
        // 奇数的时候会找中间
        ListNode slow = head;
        ListNode fast = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        // reverse slow - end of list, prev is the new head
        ListNode prev = null;
        ListNode curr = slow.next;
        slow.next = null; // the last node in the first half of list
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        
        // h
        // 1 3 
        // 2 4
        ListNode start = head;
        ListNode p1 = head; // point to the node which needs to merge in next step in list1 
        ListNode p2 = prev; // point to the node which needs to merge in next step in list2
        while (p1 != null && p2 != null) {
            ListNode nextInFirstList = p1.next;
            if (p1 != start) {
                head.next = p1;
                head = head.next;
            }
            head.next = p2;
            p1 = nextInFirstList;
            p2 = p2.next;  
            head = head.next;
        }
        if (p1 != null) {
            head.next = p1;
        }
        
        /*
        // merge 1->2->3 and 6->5->4 into 1->6->2->5->3->4
        ListNode first = head; ListNode second = prev;
        while (second.next != null) {
        完成 1->6->2的这一步在一个loop里
            2->5->3
            3->4-> null => 4.next = null, exit the loop
            temp = first.next;
            first.next = second;
            first = temp;
            
            temp = second.next;
            second.next = first;
            second = temp;
        }
        */
    }
}
```

* 注意快慢指针的slow，point to the end of the first half of list. so the start of the next half is the slow.next
* when merge two list in place, inside the loop, I did: l1: 1 -> 3 -> 5 . l2: 2 ->.4.I did 1->2
* But What else I can did is 1->2->3, 3->4->5,4.next = null, loop stop here

```
        /*
        // merge 1->2->3 and 6->5->4 into 1->6->2->5->3->4
        ListNode first = head; ListNode second = prev;
        while (second.next != null) {
        完成 1->6->2的这一步在一个loop里
            2->5->3
            3->4-> null => 4.next = null, exit the loop
            temp = first.next;
            first.next = second;
            first = temp;
            
            temp = second.next;
            second.next = first;
            second = temp;
        }
        */
```

## 82. Remove Duplicates from Sorted List II



Given the `head` of a sorted linked list, _delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list_. Return _the linked list **sorted** as well_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg)

```
Input: head = [1,2,3,3,4,4,5]
Output: [1,2,5]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/linkedlist2.jpg)

```
Input: head = [1,1,1,2,3]
Output: [2,3]
```

&#x20;

**Constraints:**

* The number of nodes in the list is in the range `[0, 300]`.
* `-100 <= Node.val <= 100`
* The list is guaranteed to be **sorted** in ascending order.

只要有duplicate就不要， 这种情况下可以找到最后一个是duplicate的node 然后直接prev.next = lastDuplicateNode.next. 需要检查current node and curr.next, 如果他们想等那么就得一直检查下去，直到找到最后一个duplicated node

```
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        // check if current node is same with the next one, if same, find all duplicate and 
        // skip them
        
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode prev = dummy;
        ListNode curr = head;
        
        while (curr != null) {
            // 如果当前的和他后面的想等的话
            if (curr.next != null && curr.val == curr.next.val) {
                // find the last duplicated node
                while (curr.next != null && curr.val == curr.next.val) {
                    curr = curr.next;
                }
                // 1 - 5 - 5 - null
                // p   c.  c 
                // 1 - null
                // skip these duplicated node
                prev.next = curr.next;
            } else {
                prev = prev.next;
            }
            curr = curr.next;
        }
        return dummy.next;
    }
}
```
