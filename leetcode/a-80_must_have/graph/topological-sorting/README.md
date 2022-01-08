# Topological Sorting

sorting the vertex in a order, in that order, the graph no backword edge

for example there is a edge u to v, after sorting, u will appear before v and v will not appear before u.&#x20;

directed graph without cycle&#x20;

But How to detect cycle exist in Topological Sorting? - check if sorting result including all nodes

### BFS:&#x20;

indegree \<Vertex, Integer> store how many edges point to this vertex

when indegree is 0, added to queue

find the neigbours of the vertex and decease its indegree by 1, if it is 0, added to queue

### DFS:

start from a point, add the last node in to res

A ->&#x20;

&#x20;           B -> C

D ->&#x20;

add C, B, A D

Time Complexity: O(Edge) -> need to go through each edge to calculate indegree

Space Complexity: O(vertex)
