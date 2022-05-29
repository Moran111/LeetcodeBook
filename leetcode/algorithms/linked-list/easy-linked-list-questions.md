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

## 876. Middle of the Linked List



Given the `head` of a singly linked list, return _the middle node of the linked list_.

If there are two middle nodes, return **the second middle** node.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg)

```
Input: head = [1,2,3,4,5]
Output: [3,4,5]
Explanation: The middle node of the list is node 3.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist2.jpg)

```
Input: head = [1,2,3,4,5,6]
Output: [4,5,6]
Explanation: Since the list has two middle nodes with values 3 and 4, we return the second one.
```

&#x20;

**Constraints:**

* The number of nodes in the list is in the range `[1, 100]`.
* `1 <= Node.val <= 100`

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
    public ListNode middleNode(ListNode head) {
        //  找比较右边的中点
        ListNode slow = head;
        ListNode fast = head;
        // 会多走一步, 因为只check下一个是不是空
        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
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

## 203. Remove Linked List Elements



Given the `head` of a linked list and an integer `val`, remove all the nodes of the linked list that has `Node.val == val`, and return _the new head_.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/06/removelinked-list.jpg)

```
Input: head = [1,2,6,3,4,5,6], val = 6
Output: [1,2,3,4,5]
```

**Example 2:**

```
Input: head = [], val = 1
Output: []
```

**Example 3:**

```
Input: head = [7,7,7,7], val = 7
Output: []
```

&#x20;

**Constraints:**

* The number of nodes in the list is in the range `[0, 104]`.
* `1 <= Node.val <= 50`
* `0 <= val <= 50`

`感觉remove的时候需要dummy variable`

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
    public ListNode removeElements(ListNode head, int val) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode prev = dummy;
        ListNode curr = head;
        
        while(head != null) {
            if (head.val == val) {
                prev.next = head.next;
                head = prev.next;
            } else {
                head = head.next;
                prev = prev.next;
            }
        }
        
        return dummy.next;
    }

```

## 234. Palindrome Linked List

Given the `head` of a singly linked list, return `true` if it is a palindrome.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```
Input: head = [1,2,2,1]
Output: true
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

```
Input: head = [1,2]
Output: false
```

&#x20;

**Constraints:**

* The number of nodes in the list is in the range `[1, 105]`.
* `0 <= Node.val <= 9`

&#x20;

**Follow up:** Could you do it in `O(n)` time and `O(1)` space?

Solution: find the mid of the linked list and reverse second part of the linked list to check if the first part is same with second part

1. find the mid node in the linked list  (slow and fast pointer). slow pointer move one step each time, and fast pointer move two step each time, different ways to write while condition will result to find different poisition of the mid point for even number of list. In this problem, we need to find the **end node of the first part of list. We want the loop stoped when f.next.next == null**

 **为什么快慢指针可以找到中点？**同化成一个路程问题，同一段路程，A的速度是B的两倍，他们同时出发，当A走完全程时，B也就刚好走过一半

```
        // while fast.next != null && fast.next.next != null
        //           f
        // 1 -> 2 -> 3 -> 4   （指向两个元素中左边的那个）
        //      s
        // 1 -> 2 -> 3 -> 4 -> 5
        //           s
        //                       f
        // while fast != null && fast.next != null
        //                     f
        // 1 -> 2 -> 3 -> 4   （指向两个元素中右边的那个）
        //           s
        // 1 -> 2 -> 3 -> 4 -> 5
        //           s
        //                       f
```

2\. Before we reverse the second part of the list, we need to make sure to set the firstHalf.next = null

```
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) {
            return true;
        }
        // solution 1
        // palindrome -> vals from start and from end are same
        // find the mid point of list, reverse one of the list and check
        
        ListNode endOfFirstHalf = null;
        // while fast.next != null && fast.next.next != null
        //           f
        // 1 -> 2 -> 3 -> 4
        //      s
        // 1 -> 2 -> 3 -> 4 -> 5
        //           s
        //                       f
        // while fast != null && fast.next != null
        //                     f
        // 1 -> 2 -> 3 -> 4
        //           s
        // 1 -> 2 -> 3 -> 4 -> 5
        //           s
        //                       f
        // find mid (two pointer) if even, left mid, right mid 
        // find the end of the first half
        ListNode slow = head;
        ListNode fast = head;
        while(fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        endOfFirstHalf = slow;
        
        // reverse linked list (if we want to reverse list, using dummy variable)
        // reverse head , endOfFirstHalf
        ListNode start = endOfFirstHalf.next;
        endOfFirstHalf.next = null; // 反转后半段，前半段不要忘记最后一个连着null
        ListNode nodeAfterReverse = reverse(start);
        // System.out.println(curr.val);
        // if it is even number 
        // first half of list should same with second half of it
        while(nodeAfterReverse != null) {
            if (head.val != nodeAfterReverse.val) {
                return false;
            }
            nodeAfterReverse = nodeAfterReverse.next;
            head = head.next;
        }
        // if odd number of list, then there is one more head left, 
        // it is ok
        return true;
    }
    
    private ListNode reverse(ListNode start) {
        ListNode prev = null;
        ListNode curr = start;
        while(curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
}
```

## 160. Intersection of Two Linked Lists

Given the heads of two singly linked-lists `headA` and `headB`, return _the node at which the two lists intersect_. If the two linked lists have no intersection at all, return `null`.

For example, the following two linked lists begin to intersect at node `c1`:

![](https://assets.leetcode.com/uploads/2021/03/05/160\_statement.png)

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

**Note** that the linked lists must **retain their original structure** after the function returns.

**Custom Judge:**

The inputs to the **judge** are given as follows (your program is **not** given these inputs):

* `intersectVal` - The value of the node where the intersection occurs. This is `0` if there is no intersected node.
* `listA` - The first linked list.
* `listB` - The second linked list.
* `skipA` - The number of nodes to skip ahead in `listA` (starting from the head) to get to the intersected node.
* `skipB` - The number of nodes to skip ahead in `listB` (starting from the head) to get to the intersected node.

The judge will then create the linked structure based on these inputs and pass the two heads, `headA` and `headB` to your program. If you correctly return the intersected node, then your solution will be **accepted**.

&#x20;

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/05/160\_example\_1\_1.png)

```
Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
Output: Intersected at '8'
Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,6,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
```

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/05/160\_example\_2.png)

```
Input: intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
Output: Intersected at '2'
Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect).
From the head of A, it reads as [1,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.
```

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/03/05/160\_example\_3.png)

```
Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
Output: No intersection
Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
Explanation: The two lists do not intersect, so return null.
```

&#x20;

**Constraints:**

* The number of nodes of `listA` is in the `m`.
* The number of nodes of `listB` is in the `n`.
* `1 <= m, n <= 3 * 104`
* `1 <= Node.val <= 105`
* `0 <= skipA < m`
* `0 <= skipB < n`
* `intersectVal` is `0` if `listA` and `listB` do not intersect.
* `intersectVal == listA[skipA] == listB[skipB]` if `listA` and `listB`intersect.

&#x20;

**Follow up:** Could you write a solution that runs in `O(m + n)` time and use only `O(1)`memory?

Imagine that we have two linked lists, A and B, and we know that their lengths are NN and MMrespectively (these can be calculated with O(1)O(1) space and in time proportional to the length of the list). We'll imagine that N=5N=5 and M=8M=8.

![Two linked lists with question marks on their nodes. The first is 5 nodes long, and the second is 8 nodes long.](https://leetcode.com/problems/Figures/160/image1.png)

Because the "tails" must be the same length, we can conclude that _if_ there is an intersection, then the intersection node will be one of these 5 possibilities.

![The two linked lists from above with arrows showing how the last 5 nodes of each list could be a match.](https://leetcode.com/problems/Figures/160/image2.png)

So, to check for each of these pairs, we would start by setting a pointer at the start of the shorter list, and a pointer at the first possible matching node of the longer list. The position of this node is simply the difference between the two lengths, that is, ∣M−N∣∣M−N∣.

![The two linked lists from above with a p1 pointer at the head of the first, and a p2 pointer at the 4th node of the second.](https://leetcode.com/problems/Figures/160/image3.png)

Then, we just need to step the two pointers through the list, each time checking whether or not the nodes are the same.

In code, we could write this algorithm with 4 loops, one after the other, each doing the following:

1. Calculate NN; the length of list A.
2. Calculate MM; the length of list B.
3. Set the start pointer for the _longer_ list.
4. Step the pointers through the list together.

While this would have a time complexity of O(N+M)O(N+M) and a space complexity of O(1)O(1)and would be fine for an interview, we can still simplify the code a bit! As some quick reassurance, most people will struggle to come up with this next part by themselves. It takes practice and seeing lots of linked list and other math problems.

**If we say that cc is the **_**shared**_** part, aa is **_**exclusive part of list A**_** and bb is **_**exclusive part of list B**_**, then we can have one pointer that goes over `a + c + b` and the other that goes over `b + c + a`. Have a look at the diagram below, and this should be fairly intuitive.**

![Diagram showing that one pointer could go over a + c + b while the other goes over b + c + a, and then both will end up on the intersection node.](https://leetcode.com/problems/Figures/160/image4.png)

This is the above algorithm in disguise - one pointer is essentially measuring the length of the longer list, and the other is measuring the length of the shorter list, and then placing the start pointer for the longer list. Then both are stepping through the list together. By seeing the solution in this way though, we can now implement it as a single loop.

```
/**
 * Definition for singly-linked list.
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
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        // two pointer, 人为的让指针循环起来
        ListNode p1 = headA;
        ListNode p2 = headB;
        while(p1 != p2) {
            p1 = p1 == null ? headB : p1.next;
            p2 = p2 == null ? headA : p2.next;
        }
        return p1;
    }
}
```
