# Minimum Spinning Tree



1135\. Connecting Cities With Minimum CostMedium66538Add to ListShare

There are `n` cities labeled from `1` to `n`. You are given the integer `n` and an array `connections` where `connections[i] = [xi, yi, costi]` indicates that the cost of connecting city `xi` and city `yi` (bidirectional connection) is `costi`.

Return _the minimum **cost** to connect all the_ `n` _cities such that there is at least one path between each pair of cities_. If it is impossible to connect all the `n` cities, return `-1`,

The **cost** is the sum of the connections' costs used.

**Example 1:**![](https://assets.leetcode.com/uploads/2019/04/20/1314\_ex2.png)

```
Input: n = 3, connections = [[1,2,5],[1,3,6],[2,3,1]]
Output: 6
Explanation: Choosing any 2 edges will connect all cities so we choose the minimum 2.
```

**Example 2:**![](https://assets.leetcode.com/uploads/2019/04/20/1314\_ex1.png)

```
Input: n = 4, connections = [[1,2,3],[3,4,4]]
Output: -1
Explanation: There is no way to connect all cities even if all edges are used.
```

**Constraints:**

* `1 <= n <= 104`
* `1 <= connections.length <= 104`
* `connections[i].length == 3`
* `1 <= xi, yi <= n`
* `xi != yi`
* `0 <= costi <= 105`



```
    Kruskal's Algorithm:
    1) Sort the edges in ascending order of their weights
    2) Start with the min weighted edge - 
        check if it doesn't perform cycle (Using Union Find)
    3) If so add to the MST. Keep adding to the running sum.

    Time: Big O(E.log.E) 

    cycle: 如果在已经形成的联通量里 连一条边，就有cycle了, 要在union的时候check
```

class Solution { int\[] parent; int totalComponent; public int minimumCost(int n, int\[]\[] connections) { // corss call, Kruskals (Union Find) /\* Kruskal's Algorithm: 1) Sort the edges in ascending order of their weights 2) Start with the min weighted edge - check if it doesn't perform cycle (Using Union Find) 3) If so add to the MST. Keep adding to the running sum.

```
    Time: Big O(E.log.E) 

    cycle: 如果在已经形成的联通量里 连一条边，就有cycle了
    */

    int cost = 0;

    // need to know current component 
    totalComponent = n;
    parent = new int[n+1];
    for (int i = 1; i <= n; i++) {
        parent[i] = i;
    }

    Arrays.sort(connections, (a, b) -> a[2] - b[2]);
    for (int[] edge: connections) {
        int node1 = edge[0];
        int node2 = edge[1];

        int px = find(node1);
        int py = find(node2);
        // if px and py is same, node1 and node2 has same parent, this means they are in same component
        if (px != py) {
            totalComponent--;
            parent[px] = py;
            cost += edge[2];
        }
    }

    return totalComponent == 1 ? cost:-1;
}

private int find(int x) {
    if (parent[x] == x) {
        return x;
    }
    parent[x] = find(parent[x]);
    return parent[x];
}
```

}
