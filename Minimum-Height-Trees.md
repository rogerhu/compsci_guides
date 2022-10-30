## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/minimum-height-trees](https://leetcode.com/problems/minimum-height-trees)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Breadth-First Search, Depth-First Search, Topological Sort
* 🗒️ **Similar Questions**: [Course Schedule](https://leetcode.com/problems/course-schedule/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What would be a N^2 solution?
  - A brute force N^2 solution would be to try every node and find its height. 

- What is a minimum height tree?
  - Minimum height tree is rooted at the mid point of the longest path in the tree. The root of minimum height tree is the middle point of the longest path in the tree; hence there are at most two minimum height tree roots. Remember a tree has n nodes and n-1 edges.

- What is the degree of the node?
  - A degree of node is basically the number of edges connected to for from the node. This is the case for an undirected graph.
        
    
```markdown
    HAPPY CASE
    Input: n = 4, edges = [[1,0],[1,2],[1,3]]
    Output: [1]
    
    Input: n = 6, edges = [[3,0],[3,1],[3,2],[3,4],[5,4]]
    Output: [3,4]
    
    EDGE CASE
    Input: n = 7, edges = [[0,1],[1,2],[1,3],[2,4],[3,5],[4,6]]
    Output: [1,2]
```
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    For graph problems, some things we want to consider are:
    
- BFS/DFS; In a tree (remember a tree with n nodes can only have n -1 edges), there can only be 1 or 2 roots for a minimum height tree. So we remove the leaves of the tree one iteration at a time. We know a node is a leaf if it has only 1 node it is connected to since this is a directed graph. If it was an undirected graph, a leaf could have 1 or 0 connected nodes. How to find a minimum height tree? We can BFS from the bottom (leaves) to the top until the last level with <=2 nodes. To build the current level from the previous level, we can monitor the degree of each node. If the node has degree of one, it will be added to the current level. Since it only check the edges once, the complexity is O(n). How will you find the longest path in a tree? Randomly select any node in the tree and find the longest path from that node. Use DFS to do that. Let the terminal node be x. x must be the end-point of the true longest path in the tree. Run DFS/BFS from x to find the real longest path in the tree.
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 
    
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:**  Filter out the roots that have the minimal height among all the trees
    
The idea is keep removing all of the leaves until there is the last layer of leaves, then those are the roots of the minimum height trees. Using an arrayList, add the first layer of leaves. Because when we break the longest branch in half, there can be at most 2 things at the top. If there are three, then we can break again. Remove all the other occurrences of this leaf from other rows that the leaf is associated with. Remove the row of the current leaf from the adjacency list.
                   

⚠️ **Common Mistakes**

*  We can approach this problem in brute-force manner by considering each one of the nodes as root and calculating the height of the tree thus formed. Each node which leads to minimum-height tree will be pushed in an array. If a node forms a tree with smaller height that observed till now, we will clear the array and refill if we find nodes having this smaller height. Finally this array will be returned. The runtime to this can be extremely slow. 
    
## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {

        // edge cases
        if (n < 2) {
            ArrayList<Integer> centroids = new ArrayList<>();
            for (int i = 0; i < n; i++)
                centroids.add(i);
            return centroids;
        }

        // Build the graph with the adjacency list
        ArrayList<Set<Integer>> neighbors = new ArrayList<>();
        for (int i = 0; i < n; i++)
            neighbors.add(new HashSet<Integer>());

        for (int[] edge : edges) {
            Integer start = edge[0], end = edge[1];
            neighbors.get(start).add(end);
            neighbors.get(end).add(start);
        }

        // Initialize the first layer of leaves
        ArrayList<Integer> leaves = new ArrayList<>();
        for (int i = 0; i < n; i++)
            if (neighbors.get(i).size() == 1)
                leaves.add(i);

        // Trim the leaves until reaching the centroids
        int remainingNodes = n;
        while (remainingNodes > 2) {
            remainingNodes -= leaves.size();
            ArrayList<Integer> newLeaves = new ArrayList<>();

            // remove the current leaves along with the edges
            for (Integer leaf : leaves) {
                // the only neighbor left for the leaf node
                Integer neighbor = neighbors.get(leaf).iterator().next();
                // remove the edge along with the leaf node
                neighbors.get(neighbor).remove(leaf);
                if (neighbors.get(neighbor).size() == 1)
                    newLeaves.add(neighbor);
            }

            // prepare for the next round
            leaves = newLeaves;
        }

        // The remaining nodes are the centroids of the graph
        return leaves;
    }
}
```
    
```python
class Solution:
    def findMinHeightTrees(self, n: int, edges: List[List[int]]) -> List[int]:

        # edge cases
        if n <= 2:
            return [i for i in range(n)]

        # Build the graph with the adjacency list
        neighbors = [set() for i in range(n)]
        for start, end in edges:
            neighbors[start].add(end)
            neighbors[end].add(start)

        # Initialize the first layer of leaves
        leaves = []
        for i in range(n):
            if len(neighbors[i]) == 1:
                leaves.append(i)

        # Trim the leaves until reaching the centroids
        remaining_nodes = n
        while remaining_nodes > 2:
            remaining_nodes -= len(leaves)
            new_leaves = []
            # remove the current leaves along with the edges
            while leaves:
                leaf = leaves.pop()
                # the only neighbor left for the leaf node
                neighbor = neighbors[leaf].pop()
                # remove the only edge left
                neighbors[neighbor].remove(leaf)
                if len(neighbors[neighbor]) == 1:
                    new_leaves.append(neighbor)

            # prepare for the next round
            leaves = new_leaves

        # The remaining nodes are the centroids of the graph
        return leaves
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: `O(V)`, where `V` represents the number of nodes in the graph
<br>
Space Complexity: `O(V)`, where `V` represents the number of nodes in the graph
