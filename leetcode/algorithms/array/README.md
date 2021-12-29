# Array

## Sort&#x20;

### Quick sort  (NLogN, O(1))

#### 215. kth largest Element

#### Median (n/2) same with 215

Merge sort (NLogN, O(N))

if it is LinkedList, then we don't need O(n) space when we doing merge ...&#x20;

Heap sort (NLogN, O(1))

## Prefix Sum

## Two Pointer

### 从两边到中间

#### 680. Valid Palindrome II



Given a string `s`, return `true` _if the_ `s` _can be palindrome after deleting **at most one** character from it_.

&#x20;

**Example 1:**

```
Input: s = "aba"
Output: true
```

**Example 2:**

```
Input: s = "abca"
Output: true
Explanation: You could delete the character 'c'.
```

**Example 3:**

```
Input: s = "abc"
Output: false
```

&#x20;

**Constraints:**

* `1 <= s.length <= 105`
* `s` consists of lowercase English letters.

```
// 需要考虑每一个case，在面试的时候，写代码前要想到
// 不好描述的时候可以具一个例子，或者多举几个例子
// 这个题比较坑的case是那种abaab
//                  　  l   r
// 这个例子如果按我的逻辑，会从左边和右边里选择一个删除
// but for this question，we can only delete the l to make the rest of the string valid 
// so this means if we reverse the s and do it again
// eigher one works, then we can return true

```
