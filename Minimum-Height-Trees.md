## Problem Highlights

🔗 **Leetcode Link:** [https://leetcode.com/problems/minimum-height-trees/description/](https://leetcode.com/problems/minimum-height-trees/description/)

⏰ **Time to complete**: __ mins

## 1. **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What would be a N^2 solution?
A brute force N^2 solution would be to try every node and find its height. 

- What is a minimum height tree?
Minimum height tree is rooted at the mid point of the longest path in the tree. The root of minimum height tree is the middle point of the longest path in the tree; hence there are at most two minimum height tree roots. Remember a tree has n nodes and n-1 edges.

- What is the degree of the node?
A degree of node is basically the number of edges connected to for from the node. This is the case for an undirected graph.
        
    
    ```markdown
    HAPPY CASE
    Input: n = 4, edges = [[1,0],[1,2],[1,3]]
    Output: [1]
    
    Input: n = 6, edges = [[3,0],[3,1],[3,2],[3,4],[5,4]]
    Output: [3,4]
    
    EDGE CASE
    Input:
    Output:
    ```
    
## 2. M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    For graph problems, some things we want to consider are:
    
    In a tree (remember a tree with n nodes can only have n -1 edges), there can only be 1 or 2 roots for a minimum height tree. So we remove the leaves of the tree one iteration at a time. We know a node is a leaf if it has only 1 node it is connected to since this is a directed graph. If it was an undirected graph, a leaf could have 1 or 0 connected nodes.
    
    How to find a minimum height tree? We can BFS from the bottom (leaves) to the top until the last level with <=2 nodes. To build the current level from the previous level, we can monitor the degree of each node. If the node has degree of one, it will be added to the current level. Since it only check the edges once, the complexity is O(n).
    
    How will you find the longest path in a tree? Randomly select any node in the tree and find the longest path from that node. Use DFS to do that. Let the terminal node be x. x must be the end-point of the true longest path in the tree. Run DFS/BFS from x to find the real longest path in the tree.
    
## 3. P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.
    
The idea is keep removing all of the leaves until there is the last layer of leaves, then those are the roots of the minimum height trees. Using an arrayList, add the first layer of leaves. Because when we break the longest branch in half, there can be at most 2 things at the top. If there are three, then we can break again. Remove all the other occurrences of this leaf from other rows that the leaf is associated with. Remove the row of the current leaf from the adjacency list.
                    
    
## 4. I-mplement

> **Implement** the code to solve the algorithm.
    
```java
    class Solution {
        
        public List<Integer> findMinHeightTrees(int n, int[][] edges) {
            if (n == 1) return Collections.singletonList(0);
            HashMap<Integer, ArrayList<Integer>> adjList = changeEdgesIntoAdjList(n, edges);
            
            ArrayList<Integer> leaves = new ArrayList<Integer>();
            
            for (int i = 0; i < n; i++) {
                if (adjList.get(i).size() == 1) {
                    leaves.add(i);
                }
            }
    
            while (n > 2) {
                n -= leaves.size();
                
                ArrayList<Integer> newLeaves = new ArrayList<Integer>();
                
                for (int leaf: leaves) {
             
                    for (int adjLeaf: adjList.get(leaf)) {
                        adjList.get(adjLeaf).remove(Integer.valueOf(leaf));
                       
                        if (adjList.get(adjLeaf).size() == 1) {
                            newLeaves.add(adjLeaf);
                        }
                    }
                    adjList.remove(leaf);
                }
                leaves = newLeaves;
            }
            
            return leaves;
        }
        
         public HashMap<Integer, ArrayList<Integer>> changeEdgesIntoAdjList(int n, int[][] edges) {
            
            HashMap<Integer, ArrayList<Integer>> adjList = new HashMap<Integer, ArrayList<Integer>>();
            
            for (int i = 0; i < n; i++) {
                adjList.put(i, new ArrayList<Integer>());
            }
            
            for (int[] edge: edges) {
                adjList.get(edge[0]).add(edge[1]);
                adjList.get(edge[1]).add(edge[0]);
            }
            
            return adjList;
        }
    }
```
    
```python
    class Solution(object):
        def findMinHeightTrees(self, n, edges):
            degree = {k:0 for k in range(n)}
            graph = {k:set([]) for k in range(n)}
            for edge in edges:
                u,v = edge[0], edge[1]
                degree[u], degree[v] = degree[u]+1, degree[v]+1
                graph[u].add(v)
                graph[v].add(u)
            leaves = [k for k,v in degree.items() if v == 1]
            while leaves and len(graph) > 2:
                for leaf in leaves:
                    v = graph[leaf].pop()
                    del graph[leaf]
                    del degree[leaf]
                    degree[v] -= 1
                    graph[v].remove(leaf)
                leaves = [k for k,v in degree.items() if v == 1]
            return list(graph.keys())
    
```
    
## 5. R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errorS and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6. E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: O(n)
<br>
Space Complexity: O(n)