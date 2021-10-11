These traversal algorithms are conceptually the same as the ones introduced in the tree section.

## Depth first search
In a **depth first search**, we start with an arbitrary node as a root and explore each neighbor fully before exploring the next one. 

<img src="https://i.imgur.com/pkvWwSF.png" width="544" height="350"/>

**Implementation:**
```python
'''
Assuming we have a directed graph represented with an adjacency list.

Example:
graph = {'A': ['B', 'C'],  
 'B': ['D', 'E'],  
 'C': ['F'],  
 'E': ['F']}
'''
  
def depth_first_search(graph, start):  
    visited, stack = set(), [start] 
    while stack:
        vertex = stack.pop()
        if vertex not in visited:
            visited.add(vertex)
            # If a node with no outgoing edges won't be 
            # included in the adjacency list, we need to check
            if vertex in graph:
                for neighbor in graph[vertex]:
                    if neighbor not in visited:
                        stack.append(neighbor)
    return visited
```

**Runtime:** O(V + E), where V = number of nodes/vertices and E = number of edges

**Example interview question using DFS:**
* [Detect a cycle in a graph](https://www.geeksforgeeks.org/detect-cycle-in-a-graph/) 

## Breadth first search
In **breadth first search**, we pick an arbitrary node as the root and explore each of its neighbors before visiting their children. Breadth first search is the better suited at finding the shortest path between two nodes.

<img src="https://i.imgur.com/S0369iR.png" width="679" height="350"/>

**Implementation:**
```python
from collections import deque

'''
Assuming we have a directed graph represented with an adjacency list.

Example:
graph = {'A': ['B', 'C'],  
 'B': ['D', 'E'],  
 'C': ['F'],  
 'E': ['F']}
'''
  
def breadth_first_search(graph, start):  
    visited, queue = set(), deque(start)
    while queue:
        vertex = queue.popleft()
        visited.add(vertex)
        # If a node with no outgoing edges won't be 
        # included in the adjacency list, we need to check
        if vertex in graph:
           for neighbor in graph[vertex]:
              if neighbor not in visited:
                 queue.append(neighbor)
    return visited
```


**Example interview question using BFS:**


**Runtime**: O(V + E), where V = number of nodes/vertices and E = number of edges

See [explanation](https://www.quora.com/Why-is-the-complexity-of-DFS-O-V+E) of why it's O(V + E).

## Key Takeaways
* DFS is better for analyzing structure of graphs (ex. looking for cycles)
* BFS is better for optimization (ex. shortest path algorithms)

## References

* https://medium.com/basecs/deep-dive-through-a-graph-dfs-traversal-8177df5d0f13
* https://medium.freecodecamp.org/deep-dive-into-graph-traversals-227a90c6a261
