# Hash & Heap & Stack

## Hash

底层存储 - open hashing and close hashing (array + linked list)

### 原理

### 应用

## Stack

最底层 - 连续性存储（Array） 和不连续性存储 （LinkedList， Tree）

Stack stack = new Stack<>(); -> this is stored in Heap memory

ListNode node = new ListNode(0). Node is the reference, the reference is stored in the stack. But Node(0) is stored in the heap memory.

when you recursively call -> use stack memory (stack overflow)

### 应用

Push O(1) / Pop O(1) / Top O(1)

非递归实现DFS的主要数据结构

## Queue

Push O(1) / Pop O(1) / Top O(1)

BFS

## Heap (PriorityQueue \&TreeMap)

底层存储  - binary search tree

### 应用

Stack & Heap&#x20;

**括号匹配是使用栈解决的经典问题。**

字符串去重问题

求逆波兰表达式

单调队列

主要思想是**队列没有必要维护窗口里的所有元素，只需要维护有可能成为窗口里最大值的元素就可以了，同时保证队列里的元素数值是由大到小的。**

那么这个维护元素单调递减的队列就叫做**单调队列，即单调递减或单调递增的队列**



#### 求前 K 个高频元素

heap

