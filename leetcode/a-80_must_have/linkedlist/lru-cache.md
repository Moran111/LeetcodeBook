---
description: Doubly Linked List - having dummy head and dummy tail
---

# 146 LRU Cache



146\. LRU CacheMedium10172400Add to ListShare

Design a data structure that follows the constraints of a [**Least Recently Used (LRU) cache**](https://en.wikipedia.org/wiki/Cache\_replacement\_policies#LRU).

Implement the `LRUCache` class:

* `LRUCache(int capacity)` Initialize the LRU cache with **positive** size `capacity`.
* `int get(int key)` Return the value of the `key` if the key exists, otherwise return `-1`.
* `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

**Example 1:**

```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
```

**Constraints:**

* `1 <= capacity <= 3000`
* `0 <= key <= 104`
* `0 <= value <= 105`
* At most 2` * 105` calls will be made to `get` and `put`.

```
class LRUCache {
    
    Map<Integer, Node> map;
    int capacity;
    Node head; // doubly linked list
    Node tail; // doubly linked list - dummy tail and dummy head
    int size;
    
    public LRUCache(int capacity) {
        this.map = new HashMap<>();
        this.capacity = capacity;
        head = new Node(-1, -1);
        tail = new Node(-1, -1);
        
        head.prev = null;
        head.next = tail;
        
        tail.next = null;
        tail.prev = head;
        this.size = 0;
    }
    
    public int get(int key) {
        // when use it, update doubly linked list
        if (map.containsKey(key)) {
            Node node = map.get(key); // remove Node from previous position
            remove(node);
            add(node);
            return node.val;
        }        
        return -1;
    }
    
    public void put(int key, int value) {        
        // if key exist, update value, use it, update in list
        if (map.containsKey(key)) {
            remove(map.get(key)); // remove prev used node
            Node node = new Node(key, value); // update value
            // map.put(key, node); 
            add(node); // put in tail
        } else {
            // check capacity
            if (size >= capacity) {
                // evict the leaset recently used key
                Node node = head.next; // least used node
                remove(node);
            }
            // put the key value to map
            Node node = new Node(key, value); // update value
            // map.put(key, node); 
            add(node); // put in tail
        }
    }
    
    // add to one before tail
    private void add(Node node) {
        // it is head
        Node prev = tail.prev;
        prev.next = node;
        node.prev = prev;
        
        node.next = tail;
        tail.prev = node;
        
        // also add in map
        map.put(node.key, node); 
        size++;
    }
    
    private void remove(Node node) {           

        Node prev = node.prev;
        Node next = node.next;
        prev.next = next;
        next.prev = prev;

        // also removed it from map
        map.remove(node.key);
        size--;
    }
}

class Node {
    int key;
    int val;
    Node prev;
    Node next;
    public Node (int key, int val) {
        this.key = key;
        this.val = val;
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```
