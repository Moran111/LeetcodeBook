# Reorder Linked List Solved

You are given the head of a singly linked-list.

The positions of a linked list of `length = 7` for example, can intially be represented as:

`[0, 1, 2, 3, 4, 5, 6]`

Reorder the nodes of the linked list to be in the following order:

`[0, 6, 1, 5, 2, 4, 3]`

Notice that in the general case for a list of `length = n` the nodes are reordered to be in the following order:

`[0, n-1, 1, n-2, 2, n-3, ...]`

You may not modify the values in the list's nodes, but instead you must reorder the nodes themselves.

**Example 1:**

```java
Input: head = [2,4,6,8]

Output: [2,8,4,6]
```

**Example 2:**

```java
Input: head = [2,4,6,8,10]

Output: [2,10,4,8,6]
```

**Constraints:**

* `1 <= Length of the list <= 1000`.
* `1 <= Node.val <= 1000`

// find mid, reverse second part\
// merge two list\
// first part of array should have 1 more elemnt than right part of array

when the list has odd number of elements, after we split to 2 parts of elements, we need to make sure left part has 1 more element than right part.

So we need to use following code get the mid.&#x20;

ListNode slow = head;\
ListNode fast = head.next;\
这跟常见的写法（fast = head）略有不同，因此它会改变“中点”的定义 —— 尤其是在链表长度为偶数时。

```
slow 每次走一步
fast 每次走两步
当 fast == null || fast.next == null 时，slow 停止
初始时 fast 从 head.next 开始，导致 slow “领先一步” 停止
```

#### ✅ 看两个例子：

**🟦 例子 1：奇数长度（n = 5）**

链表：`1 → 2 → 3 → 4 → 5`

* 初始：`slow = 1`, `fast = 2`
* 第一步：`slow = 2`, `fast = 4`
* 第二步：`slow = 3`, `fast = null` → 停止

此时：

* `slow` 停在了第3个节点
* 左边是：`1, 2, 3`（3个元素）
* 右边是：`4, 5`（2个元素）

✅ 左边比右边多1个

***

**🟦 例子 2：偶数长度（n = 4）**

链表：`1 → 2 → 3 → 4`

* 初始：`slow = 1`, `fast = 2`
* 第一步：`slow = 2`, `fast = 4`
* 第二步：`fast.next == null` → 停止

此时：

* `slow` 停在第2个节点
* 左边是：`1, 2`（2个元素）
* 右边是：`3, 4`（2个元素）

✅ 左右相等（不是多1个）
