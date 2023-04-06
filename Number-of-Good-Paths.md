## Problem Highlights

* 🔗 **Leetcode Link:** [Number of Good Paths](https://leetcode.com/problems/number-of-good-paths/)
* 💡 **Problem Difficulty:** Hard
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Graph 
* 🗒️ **Similar Questions**: [Checking Existence of Edge Length Limited Paths](https://leetcode.com/problems/checking-existence-of-edge-length-limited-paths/), [Longest Nice Substring](https://leetcode.com/problems/longest-nice-substring/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What are the constraints relevant to this problem?

```markdown
Constraints:

n == vals.length
1 <= n <= 3 * 104
0 <= vals[i] <= 105
edges.length == n - 1
edges[i].length == 2
0 <= ai, bi < n
ai != bi
edges represents a valid tree.
```   


```markdown
Example 1:
Input: vals = [1,3,2,1,3], edges = [[0,1],[0,2],[2,3],[2,4]]
Output: 6

Example 2: 
Input: vals = [1,1,2,2,3], edges = [[0,1],[1,2],[2,3],[2,4]]
Output: 7
Explanation: There are 5 good paths consisting of a single node.
There are 2 additional good paths: 0 -> 1 and 2 -> 3.

Example 3: 
Input: vals = [1], edges = []
Output: 1
Explanation: The tree consists of only one node, so there is one good path.
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
1. Create an adjacency list where adj[X] contains all the neighbors of node X.
2. Create a map valuesToNodes where valuesToNodes[X] is an array that contains all the nodes having the value X. The data structure chosen to create such a map sorts the keys in non-decreasing order, i.e., the keys are sorted.
3. Iterate over all the nodes and add each node to valuesToNodes[vals[node]].
4. Create a class UnionFind defining standard methods find and union_set.
5. Create an instance of UnionFind, passing the size as n. Also, initialize the count of good paths variable goodPaths with 0.

6. Iterate over each entry value, nodes in valuesToNodes in ascending order.
For every node in nodes, iterate over its neighbors.
For each neighbor of the node, if vals[node] >= vals[neighbor] we perform a union of the node with the neighbor.
After iterating through all the nodes, we create a map group. group[A] contains the number of nodes (from the nodes array) that belong to the same component A. For every node in nodes, we find its component and increment the size of that component by 1 in groups, i.e., group[find(node)] = group[find(node)] + 1.
We iterate through all the entries in the group and, for each key, get the value called size corresponding to it. Add (size * (size + 1) / 2) to the goodPaths.

7. Return goodPaths.
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



