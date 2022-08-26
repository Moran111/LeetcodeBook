# Normal Stack

## 232. Implement Queue using Stacks

mplement a first in first out (FIFO) queue using only two stacks. The implemented queue should support all the functions of a normal queue (`push`, `peek`, `pop`, and `empty`).

Implement the `MyQueue` class:

* `void push(int x)` Pushes element x to the back of the queue.
* `int pop()` Removes the element from the front of the queue and returns it.
* `int peek()` Returns the element at the front of the queue.
* `boolean empty()` Returns `true` if the queue is empty, `false` otherwise.

**Notes:**

* You must use **only** standard operations of a stack, which means only `push to top`, `peek/pop from top`, `size`, and `is empty` operations are valid.
* Depending on your language, the stack may not be supported natively. You may simulate a stack using a list or deque (double-ended queue) as long as you use only a stack's standard operations.

&#x20;

**Example 1:**

<pre><code>Input
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
<strong>Output
</strong>[null, null, null, 1, 1, false]
<strong>Explanation
</strong>MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false</code></pre>

&#x20;

**Constraints:**

* `1 <= x <= 9`
* At most `100` calls will be made to `push`, `pop`, `peek`, and `empty`.
* All the calls to `pop` and `peek` are valid.

**Follow-up:** Can you implement the queue such that each operation is [**amortized**](https://en.wikipedia.org/wiki/Amortized\_analysis) `O(1)` time complexity? In other words, performing `n`operations will take overall `O(n)` time even if one of those operations may take longer.

用两个stack模拟queue

用两个stack模拟queue的话，queue是first in first out. 所以stack里就要保持相反的顺序，queue：1 2 3 4. Stack： 4 3 2 1.  当我们加5进去stack的时候，4 3 2 1 5. 我们需要另一个stack形成1 2 3 4 5，然后pop到另一个stack里，变成 5 4 3 2 1.

```
class MyQueue {
    // queue first in first out
    // stack first in last out
    // 1 2 3 4 
    // s1: 
    // s2: 4 3 2 
    Deque<Integer> s1;
    Deque<Integer> s2;
    
    public MyQueue() {
        s1 = new ArrayDeque();
        s2 = new ArrayDeque();
    }
    
    public void push(int x) {
        // pop non empty stack's element to empty stack
        // s1:  
        // s2: 
        // s1: 2 3 4
        // s2: (4 3 2)
        // push to empty stack
        // s1: 2 3 4
        // s2: 5
        // pop another stack's element to previous stack
        // s1:
        // s2: 5 4 3 2
        if (s1.isEmpty()) {
           while(!s2.isEmpty()) {
             s1.addFirst(s2.removeFirst());
            } 
        } else {
            while(!s1.isEmpty()) {
                s2.addFirst(s1.removeFirst());
            }
        }
        if (s1.isEmpty()) {
            s1.addFirst(x);
            while(!s2.isEmpty()) {
             s1.addFirst(s2.removeFirst());
            } 
        } else {
            s2.addFirst(x);
            while(!s1.isEmpty()) {
             s2.addFirst(s1.removeFirst());
            } 
        }
    }
    
    public int pop() {
        if (s1.isEmpty()) {
            return s2.removeFirst();
        } else {
            return s1.removeFirst();
        }
    }
    
    public int peek() {
        if (s1.isEmpty()) {
            return s2.peekFirst();
        } else {
            return s1.peekFirst();
        }
    }
    
    public boolean empty() {
        return s1.isEmpty() && s2.isEmpty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

I have one input stack, onto which I push the incoming elements, and one output stack, from which I peek/pop. I move elements from input stack to output stack when needed, i.e., when I need to peek/pop but the output stack is empty. When that happens, I move all elements from input to output stack, thereby reversing the order so it's the correct order for peek/pop.

queue and stack have reverse order. So when we call peek or pop, we can convert input stack to reverse order -> in output stack.&#x20;

as long as we have output stack, we can keep pop from it (output stack has correct order). We can put input stack in reverse order in output stack when output stack is empty.

Lets take one example =>\
(1.) push 1 to 5 (2.) pop (3.)push 6 to 8

1. ```
     I/P  5 4 3 2 1      O/P 
   ```
2. ```
     I/P                      O/P  1 2 3 4 5 
     I/P                      O/P  2 3 4 5
   ```
3. ```
     I/P  6 7 8            O/P  2 3 4 5 
   ```

when output stack is completely empty then push to output stack.

```
class MyQueue {
    // 1 2 
    Deque<Integer> input = new ArrayDeque<>();
    Deque<Integer> output = new ArrayDeque<>();
    
    public MyQueue() {
        
    }
    
    public void push(int x) {
        input.addFirst(x);
    }
    
    public int pop() {
        peek();
        return output.removeFirst();
    }
    
    public int peek() {
        if (output.isEmpty()) {
            while(!input.isEmpty()) {
                output.addFirst(input.removeFirst());
            }
        }
        return output.peekFirst();
    }
    
    public boolean empty() {
        return input.isEmpty() && output.isEmpty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

## 155. Min Stack



## 225. [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues)

