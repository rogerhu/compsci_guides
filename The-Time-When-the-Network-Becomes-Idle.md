## Problem Highlights

* 🔗 **Leetcode Link:** [The Time When the Network Becomes Idle](https://leetcode.com/problems/the-time-when-the-network-becomes-idle/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Graph 
* 🗒️ **Similar Questions**: [Network Delay Time](https://leetcode.com/problems/network-delay-time/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- 

```markdown

```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Graph Problems, common solution patterns include:


- DFS/BFS: We could use either BFS or DFS. DFS is fewer lines of code, but BFS makes use of the adjacency dictionary data structure.
- Adjacency List: We already have an adjacency list, let's make it a adjacency dictionary.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but this will make the problem more complicated.
- Topological Sort: In order to have a topological sorting, the graph must not contain any cycles. We cannot apply this sort to this problem because we can have cycles in our graph.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.


```markdown

```


⚠️ **Common Mistakes**

* 
 
## 4: I-mplement

> **Implement** the code to solve the algorithm.


```python

```

```java

```


## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `V` represents the number of vertices/nodes.
Assume `E` represents the number of edges

* **Time Complexity**: O(V+E) We may need to visit each vertex/nodes and their edges.
* **Space Complexity**: O(V+E) accounting for the use of a adjacency dictionary.



