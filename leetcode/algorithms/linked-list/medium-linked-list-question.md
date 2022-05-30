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

