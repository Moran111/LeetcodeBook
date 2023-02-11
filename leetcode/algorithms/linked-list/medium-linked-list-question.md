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

given the index of the node, reverse them.&#x20;

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

## 19. Remove Nth Node From End of List

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/remove\_ex1.jpg)

```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

**Example 2:**

```
Input: head = [1], n = 1
Output: []
```

**Example 3:**

```
Input: head = [1,2], n = 1
Output: [1]
```

&#x20;

**Constraints:**

* The number of nodes in the list is `sz`.
* `1 <= sz <= 30`
* `0 <= Node.val <= 100`
* `1 <= n <= sz`

**Follow up:** Could you do this in one pass?

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        int count = 1;
        ListNode curr = head;
        while(curr.next != null) {
            curr = curr.next;
            count++;
        }
        
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        
        int lastIndexBeforeRemove = count - n;
        ListNode prev = dummy;
        for (int i = 0; i < lastIndexBeforeRemove; i++) {
            prev = prev.next;
        }
        
        if (prev.next != null) {
            ListNode next = prev.next.next;
            prev.next = next;
        } else {
            prev.next = null;
        }
           
        return dummy.next;
    }
}
```

如果one pass的话，就让slow和fast之间差n个，这样 fast到最后一个node的时候，slow就会在n的倒数的n的前一个

d - 1 - 2 -3 - 4- 5 - null (n = 2)

s

&#x20;                f&#x20;

&#x20;                s                f

```
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode slow = dummy;
        ListNode fast = dummy;
        
        // move n + 1 times, so there are n gaps between slow and fast
        for (int i = 1; i <= n + 1; i++) {   
            fast = fast.next;
        }
        
        while (fast != null) {
            slow = slow.next;
            fast = fast.next;
        }
        
        ListNode next = slow.next.next;
        slow.next = next;
        return dummy.next;
    }
}
```



## 148. Sort Linked List

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

### Merge Sort NlogN, O(1) space for linked list, but O(n) space for Array.&#x20;

don't remeber write base case for recursion

```
class Solution {
    public ListNode sortList(ListNode head) {
       return mergeSort(head);        
    }
    
    public ListNode mergeSort(ListNode head) {
        // base case
        if (head == null || head.next == null) {
            return head;
        }
         // find mid 
        ListNode slow = head;
        ListNode fast = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        ListNode secondPart = slow.next;
        slow.next = null;
          
        ListNode l1 = mergeSort(head);
        ListNode l2 = mergeSort(secondPart);
        
        /*
        [4,2,1,3]
        4 2 -> 2, 4
        1 3 -> 1, 3
        4 1 

        */
        // System.out.println(l1.val + " " + l2.val);
        return merge(l1, l2);
    }
    
    public ListNode merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(-1);
        ListNode head = dummy;
        ListNode h1 = l1;
        ListNode h2 = l2;
        while(h1 != null && h2 != null) {
            if (h1.val <= h2.val) {
                head.next = h1;
                h1 = h1.next;
            } else {
                head.next = h2;
                h2 = h2.next;
            }
            head = head.next;
        }
        while (h1 != null) {
            head.next = h1;
            h1 = h1.next;
            head = head.next;
        }
        while (h2 != null) {
            head.next = h2;
            h2 = h2.next;
            head = head.next;
        }
        return dummy.next;
    }
}
```

## 86. Partition List



Given the `head` of a linked list and a value `x`, partition it such that all nodes **less than** `x`come before nodes **greater than or equal** to `x`.

You should **preserve** the original relative order of the nodes in each of the two partitions.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/partition.jpg)

```
Input: head = [1,4,3,2,5,2], x = 3
Output: [1,2,2,4,3,5]
```

**Example 2:**

```
Input: head = [2,1], x = 2
Output: [1,2]
```

&#x20;

**Constraints:**

* The number of nodes in the list is in the range `[0, 200]`.
* `-100 <= Node.val <= 100`
* `-200 <= x <= 200`

```
class Solution {
    public ListNode partition(ListNode head, int x) {
        // partition 
        ListNode beforeHead = new ListNode(-1);
        ListNode before = beforeHead;
        ListNode after = new ListNode(-1);
        ListNode afterHead = after;
        
        while (head != null) {
            if (head.val < x) {
                before.next = head;
                before = before.next;
            } else {
                after.next = head;
                after = after.next;
            }
            head = head.next;
        }
        after.next = null;
        
        before.next = afterHead.next; 
        return beforeHead.next;
    }
}
```

## 61. Rotate List



Given the `head` of a linked list, rotate the list to the right by `k`places.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/rotate1.jpg)

```
Input: head = [1,2,3,4,5], k = 2
Output: [4,5,1,2,3]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/13/roate2.jpg)

```
Input: head = [0,1,2], k = 4
Output: [2,0,1]
```

&#x20;

**Constraints:**

* The number of nodes in the list is in the range `[0, 500]`.
* `-100 <= Node.val <= 100`
* `0 <= k <= 2 * 109`

```
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        // find the last k / size node newStart
        // add newStart before head
        // 如果size是k的整数倍数的时候，rotate之后还是不会变
        if (head == null || head.next == null || k == 0) {
            return head;
        }
        
        ListNode start = head;
        ListNode tail = head;
        int size = 1;
        while (tail.next != null) {
            size++;
            tail = tail.next;
        }
        
        if (k % size == 0) {
            return head;
        }
        
        ListNode curr = head;
        int newStartIndex = k % size;
        // if (k < size) {
        //     newStartIndex = k % size;
        // } else {
        //     newStartIndex = k / size;
        // }
        for (int i = 1; i < size - newStartIndex; i++) {
            curr = curr.next;
        }
        
        ListNode newStart = curr.next;
        curr.next = null;
        
        tail.next = head;
        return newStart;
    }
```

## 142. Linked List Cycle II

Given the `head` of a linked list, return _the node where the cycle begins. If there is no cycle, return_ `null`.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist\_test2.png)

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist\_test3.png)

```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

&#x20;

**Constraints:**

* The number of the nodes in the list is in the range `[0, 104]`.
* `-105 <= Node.val <= 105`
* `pos` is `-1` or a **valid index** in the linked-list.

&#x20;

**Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?

Use the diagram below to help understand the proof of this approach's correctness.

![Phase 2 diagram](https://leetcode.com/problems/Figures/142/diagram.png)

We can harness the fact that `hare` moves twice as quickly as `tortoise` to assert that when `hare` and `tortoise` meet at node hh, `hare` has traversed twice as many nodes. Using this fact, we deduce the following:

To compute the intersection point, let's note that the hare has traversed twice as many nodes as the tortoise, _i.e._ 2d(tortoise)=d(hare)2d(tortoise)=d(hare), that means

2(F+a)=F+nC+a2(F+a)=F+nC+a, where nn is some integer.

> Hence the coordinate of the intersection point is F+a=nCF+a=nC.

所以交点到cycle开始的点的距离 等于head到a的点的距离，所以变成p1 point head, p2 point intersection point, find the node they meet, this node will be the cycle start point

```
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode cycleStart = isIntersect(head);
       if (cycleStart == null) {
           return null;
       }
        
        ListNode p1 = head;
        ListNode p2 = cycleStart;
        while (p1 != p2) {
            p1 = p1.next;
            p2 = p2.next;
        }
        
        return p1;
    }
    
    public ListNode isIntersect(ListNode head) {
         ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                return slow;
            }
        }
        return null;
    }
}
```



## 430. Flatten a Multilevel Doubly Linked List

You are given a doubly linked list, which contains nodes that have a next pointer, a previous pointer, and an additional **child pointer**. This child pointer may or may not point to a separate doubly linked list, also containing these special nodes. These child lists may have one or more children of their own, and so on, to produce a **multilevel data structure** as shown in the example below.

Given the `head` of the first level of the list, **flatten** the list so that all the nodes appear in a single-level, doubly linked list. Let `curr` be a node with a child list. The nodes in the child list should appear **after** `curr` and **before** `curr.next` in the flattened list.

Return _the_ `head` _of the flattened list. The nodes in the list must have **all** of their child pointers set to_ `null`.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/11/09/flatten11.jpg)

```
Input: head = [1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
Output: [1,2,3,7,8,11,12,9,10,4,5,6]
Explanation: The multilevel linked list in the input is shown.
After flattening the multilevel linked list it becomes:
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/11/09/flatten2.1jpg)

```
Input: head = [1,2,null,3]
Output: [1,3,2]
Explanation: The multilevel linked list in the input is shown.
After flattening the multilevel linked list it becomes:
```

**Example 3:**

```
Input: head = []
Output: []
Explanation: There could be empty list in the input.
```

&#x20;

**Constraints:**

* The number of Nodes will not exceed `1000`.
* `1 <= Node.val <= 105`

&#x20;

**How the multilevel linked list is represented in test cases:**

We use the multilevel linked list from **Example 1** above:

```
 1---2---3---4---5---6--NULL
         |
         7---8---9---10--NULL
             |
             11--12--NULL
```

The serialization of each level is as follows:

```
[1,2,3,4,5,6,null]
[7,8,9,10,null]
[11,12,null]
```

To serialize all levels together, we will add nulls in each level to signify no node connects to the upper node of the previous level. The serialization becomes:

```
[1,    2,    3, 4, 5, 6, null]
             |
[null, null, 7,    8, 9, 10, null]
                   |
[            null, 11, 12, null]
```

Merging the serialization of each level and removing trailing nulls we obtain:

```
[1,2,3,4,5,6,null,null,null,7,8,9,10,null,null,11,12]
```

递归做，递归的定义，接收的是一个node，返回的是flattern之后的tail。在falttern的时候要先

1. 把child连在head的next上，然后清空child
2. 递归找到child的最后一个tail和next的最后一个tail
3. 如果有child和next，child.next = original (head.next) 且返回next。tail
4. 如果只有child 返回child的tail
5. 如果只有next，返回next的tail

```
class Solution {
    public Node flatten(Node head) {
        // base case
        
        helper(head);
        
        return head;
    }
    
    // return the last node of the child
    private Node helper(Node head) {
        if (head == null || (head.child == null && head.next == null)) {
            return head;
        }
        
        Node childNode = head.child;
        Node nextNode = head.next;
        //把child连到head上，然后把child设置成null
        if (childNode != null) {
            head.next = childNode;
            childNode.prev = head;
            head.child = null;
        }
        
        //对当前的child和当前的next分别递归，返回next的最后
        Node childTail = helper(childNode);
        Node nextTail = helper(nextNode);
        
        //分情况讨论返回什么
        if (childNode != null && nextNode != null) { 
            childTail.next = nextNode;
            nextNode.prev = childTail;
            return nextTail;
        }
        
        if (childNode == null) {
            return nextTail;
        }
        return childTail;
    }
    
    // head.child == null && head.next != null, return head.next
    // head.child != null && head.next == null, return head.child
    // head.child == null && head.next == null, return head
}
```

## 25. Reverse Nodes in k-Group

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return _the modified list_.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse\_ex1.jpg)

```
Input: head = [1,2,3,4,5], k = 2
Output: [2,1,4,3,5]
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse\_ex2.jpg)

```
Input: head = [1,2,3,4,5], k = 3
Output: [3,2,1,4,5]
```

&#x20;

**Constraints:**

* The number of nodes in the list is `n`.
* `1 <= k <= n <= 5000`
* `0 <= Node.val <= 1000`

&#x20;

**Follow-up:** Can you solve the problem in `O(1)` extra memory space?

如果少于k个的话，就不要reverse了

需要知道reverse开始前的最后一个node，reverse开始的node，和reverse完成后的tail

```
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        int count = k;
        
        ListNode temp = dummy; 
        // last node before start reversing point
        ListNode curr = head;
        while(curr != null) {
            // check how many node left
            ListNode p1 = curr;
            int size = 0;
            while (p1 != null) {
                p1 = p1.next;
                size++;
            }
            // if less than k nodes, don't rotate
            if (size < k) {
                return dummy.next;
            }
            
            ListNode prev = null;
            ListNode currStart = curr;   
            while (curr != null && count > 0) {
                count--;
                ListNode next = curr.next;
                curr.next = prev;
                prev = curr;
                curr = next;
            }
            
            // after reverse: currStart(tail) <-   prev <- curr(nextStart)
            
            //System.out.println(currStart.val + " " + " " + prev.val);
            
            temp.next = prev;
            currStart.next = curr; // curr is the next start point

            temp = currStart; // the dummy of next rotation
            count = k;
        }
        return dummy.next;
    }
}
```

## 1669. Merge in Between Linked Lists



You are given two linked lists: `list1` and `list2` of sizes `n` and `m` respectively.

Remove `list1`'s nodes from the `ath` node to the `bth` node, and put `list2` in their place.

The blue edges and nodes in the following figure indicate the result:

![](https://assets.leetcode.com/uploads/2020/11/05/fig1.png)

_Build the result list and return its head._

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/05/merge\_linked\_list\_ex1.png)

```
Input: list1 = [0,1,2,3,4,5], a = 3, b = 4, list2 = [1000000,1000001,1000002]
Output: [0,1,2,1000000,1000001,1000002,5]
Explanation: We remove the nodes 3 and 4 and put the entire list2 in their place. The blue edges and nodes in the above figure indicate the result.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/05/merge\_linked\_list\_ex2.png)

```
Input: list1 = [0,1,2,3,4,5,6], a = 2, b = 5, list2 = [1000000,1000001,1000002,1000003,1000004]
Output: [0,1,1000000,1000001,1000002,1000003,1000004,6]
Explanation: The blue edges and nodes in the above figure indicate the result.
```

&#x20;

**Constraints:**

* `3 <= list1.length <= 104`
* `1 <= a <= b < list1.length - 1`
* `1 <= list2.length <= 104`

```
class Solution {
    public ListNode mergeInBetween(ListNode list1, int a, int b, ListNode list2) {
        ListNode dummy = new ListNode(-1);
        dummy.next = list1;
        
        ListNode head = list1;
        int count = 0;
        
        ListNode start = null;
        ListNode end = null;
        
        while (count < a) {
            start = head;
            head = head.next;
            count++;
        }
        
        while (count < b) {
            end = head;
            head = head.next;
            count++;
        }
        
        ListNode tail = list2;
        while (tail.next != null) {
            tail = tail.next;
        }
        
        start.next = list2;
        tail.next = head.next;
        
        return dummy.next;
    }
}
```

## 725. Split Linked List in Parts



Given the `head` of a singly linked list and an integer `k`, split the linked list into `k`consecutive linked list parts.

The length of each part should be as equal as possible: no two parts should have a size differing by more than one. This may lead to some parts being null.

The parts should be in the order of occurrence in the input list, and parts occurring earlier should always have a size greater than or equal to parts occurring later.

Return _an array of the_ `k` _parts_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/06/13/split1-lc.jpg)

```
Input: head = [1,2,3], k = 5
Output: [[1],[2],[3],[],[]]
Explanation:
The first element output[0] has output[0].val = 1, output[0].next = null.
The last element output[4] is null, but its string representation as a ListNode is [].
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/06/13/split2-lc.jpg)

```
Input: head = [1,2,3,4,5,6,7,8,9,10], k = 3
Output: [[1,2,3,4],[5,6,7],[8,9,10]]
Explanation:
The input has been split into consecutive parts with size difference at most 1, and earlier parts are a larger size than the later parts.
```

&#x20;

**Constraints:**

* The number of nodes in the list is in the range `[0, 1000]`.
* `0 <= Node.val <= 1000`
* `1 <= k <= 50`

怎么能得倒想要的node前的那一个，用一个prev指针

```
class Solution {
    public ListNode[] splitListToParts(ListNode head, int k) {
        ListNode[] lists = new ListNode[k];
        
        // if size < k, each bucket put one node and left others empty
        // size of list, use k / size and put reminders back from the start bucket
        
        int size = 0;
        ListNode temp = head;
        while (temp != null) {
            temp = temp.next;
            size++;
        }
        
        // if (size <= k) {
        //     ListNode temp = head;
        //     for (int i = 0; i < k; i++) {
        //         if (temp == null) {
        //             continue;
        //         }
        //         ListNode next = temp.next;
        //         temp.next = null;
        //         lists[i] = temp;
        //         temp = next;
        //     }
        //     return lists;
        // }
        
        int val = size / k;
        int reminder = size % k;
    
        ListNode prev = null;
        ListNode curr = head;
        for (int i = 0; i < k; i++) {
            // put val number of nodes
            ListNode start = curr;      // HOW TO GET THE NODE BEFORE I WANT
            int c = 0;
            while (curr != null && c < val) {
                prev = curr;
                curr = curr.next;
                c ++;
            }
            if (reminder > 0) {
                prev = curr; //4
                curr = curr.next; //5
                reminder--;
            }
            // if prev == null, means no nodes to put
            if (prev != null) {
                prev.next = null;
            }
            
            ListNode next = curr;
            
            lists[i] = start;
        
            prev = curr;
        }  
        
        return lists;
    }
}
```

## 109. Convert Sorted List to Binary Search Tree



Given the `head` of a singly linked list where elements are **sorted in ascending order**, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of _every_ node never differ by more than 1.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/17/linked.jpg)

```
Input: head = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: One possible answer is [0,-3,9,-10,null,5], which represents the shown height balanced BST.
```

**Example 2:**

```
Input: head = []
Output: []
```

&#x20;

**Constraints:**

* The number of nodes in `head` is in the range `[0, 2 * 104]`.
* `-105 <= Node.val <= 105`

找mid的点，然后把mid的点左半边和右半边分别递归

```
class Solution {
    public TreeNode sortedListToBST(ListNode head) {
        // get the root at index i
        // build left subtree and right subtree [0, i-1] [i+1, end]
        
        if (head == null) {
            return null;
        }
        
        if (head.next == null) {
            return new TreeNode(head.val);
        }
        
        // get the mid of linked list - as the root
        ListNode prev = null;
        ListNode slow = head;
        ListNode fast = head;
        while (fast != null && fast.next != null) {
            prev = slow;
            slow = slow.next;
            fast = fast.next.next;
        }
        
        // System.out.println(prev.val);
        ListNode mid = prev.next;
        TreeNode root = new TreeNode(mid.val);
        
        prev.next = null;
        ListNode left = head;
        ListNode right = mid.next;
        
        
        TreeNode leftNode = sortedListToBST(left);
        TreeNode rightNode = sortedListToBST(right);
        
        if (leftNode != null) {
             root.left = leftNode;
        }
        
        if (rightNode != null) {
            root.right = rightNode;
        }
     
        return root;
    }
}
```

## 24. Swap Nodes in Pairs



Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/swap\_ex1.jpg)

```
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

**Example 2:**

```
Input: head = []
Output: []
```

**Example 3:**

```
Input: head = [1]
Output: [1]
```

&#x20;

**Constraints:**

* The number of nodes in the list is in the range `[0, 100]`.
* `0 <= Node.val <= 100`

```
class Solution {
    public ListNode swapPairs(ListNode head) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        
        ListNode prev = dummy;
        ListNode curr = head;
        // p c c.n
        //   1 2    3   4 
        //          n
        // p.next = 2
        // 2.next = 1
        
        while (curr != null && curr.next != null) {
            ListNode firstNode = curr;
            ListNode secondNode = curr.next;
            
            prev.next = secondNode; // make firstNode not connect to others
            // p - 1 - 2 - 3
            // p - 2   1 - 3
            firstNode.next = secondNode.next;
            secondNode.next = firstNode;
            
            prev = firstNode;
            curr = firstNode.next;
        }
        
        return dummy.next;
    }
}
```
