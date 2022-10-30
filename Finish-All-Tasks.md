## Problem Highlights

* 🔗 **Leetcode Link:** [https://www.geeksforgeeks.org/find-whether-it-is-possible-to-finish-all-tasks-or-not-from-given-dependencies](https://www.geeksforgeeks.org/find-whether-it-is-possible-to-finish-all-tasks-or-not-from-given-dependencies)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Depth-First Search
* 🗒️ **Similar Questions**: [Find Minimum Time to Finish All Jobs II](https://leetcode.com/problems/find-minimum-time-to-finish-all-jobs-ii/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How do we encode the prerequisites into a graph?
  - You can denote each task as a node and the prerequisite relationship as a one-direction edge.

- When is it impossible for you to finish all tasks?
  - When there exists at least one task pair t1 and t2, such that t1 is direct or indirect prerequisite for t2 and t2 is direct or indirect prerequisite for t1. This is equivalent to finding a cycle in our graph representation.
    
```markdown
    HAPPY CASE
    Input: 2, [[1, 0]] 
    Output: true 
    
    Input: 2, [[1, 0], [0, 1]] 
    Output: false 
    
    EDGE CASE
    Input: 4, [[1,0],[2,0],[3,1],[3,2]]
    Output: true
```
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
- BFS: BFS uses the indegrees of each node. We will first try to find a node with 0 indegree. If we fail to do so, there must be a cycle in the graph and we return false. Otherwise we set its indegree to be -1 to prevent from visiting it again and reduce the indegrees of its neighbors by 1. This process will be repeated for n (number of nodes) times.
- DFS: All tasks are nodes of the graph and if task u is a prerequisite of task v, we will add a directed edge from node u to node v. Now, this problem is equivalent to detecting a cycle in the graph represented by prerequisites. If there is a cycle in the graph, then it is not possible to finish all tasks (because in that case there is no any topological order of tasks). Both BFS and DFS can be used to solve it.
Since pair is inconvenient for the implementation of graph algorithms, we first transform it to a graph. If task u is a prerequisite of task v, we will add a directed edge from node u to node v.
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.
- Map: We can use a map to store the edges in the graph to lookup equations by name, and store all neighbors by name.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 
    
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Generate all possible tasks distribution among workers and compare with previous best result
    
```
1) in each visit, we start from a node and keep visiting its neighbors, 
2) if at a time we return to a visited node, there is a cycle. Otherwise, start again from another unvisited node and repeat this process. 
```
    
⚠️ **Common Mistakes**

* Each node is visited more than once.
* Using Kahn's algorithm, you can peel off the nodes with `indegree` 0, rather than nodes with `outdegree` 0.

## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
    // returns adjacency list representation from a list of pairs
    static ArrayList<ArrayList<Integer>> make_graph(int numTasks, Vector<pair> prerequisites) {
        ArrayList<ArrayList<Integer>> graph = new ArrayList<ArrayList<Integer>>(numTasks);
 
        for(int i=0; i<numTasks; i++){
            graph.add(new ArrayList<Integer>());
        }
 
        for (pair pre : prerequisites)
            graph.get(pre.second).add(pre.first);
 
        return graph;
    }
     
    // a DFS based function to check if there is a cycle in the directed graph
    static boolean dfs_cycle(ArrayList<ArrayList<Integer>> graph, int node, boolean onpath[], boolean visited[]) {
        if (visited[node])
            return false;
        onpath[node] = visited[node] = true;
 
        for (int neigh : graph.get(node))
            if (onpath[neigh] || dfs_cycle(graph, neigh, onpath, visited))
                return true;
 
        return onpath[node] = false;
    }
     
    // main function to check whether possible to finish all tasks or not
    static boolean canFinish(int numTasks, Vector<pair> prerequisites) {
        ArrayList<ArrayList<Integer>> graph = make_graph(numTasks, prerequisites);
         
        boolean onpath[] = new boolean[numTasks];
        boolean visited[] = new boolean[numTasks];
 
        for (int i = 0; i < numTasks; i++)
            if (!visited[i] && dfs_cycle(graph, i, onpath, visited))
                return false;
 
        return true;
    }
```
    
```python
# returns adjacency list representation from a list of pairs
def make_graph(numTasks, prerequisites):
    graph = []
    for i in range(numTasks):
        graph.append([])
 
    for pre in prerequisites:
        graph[pre.second].append(pre.first)
 
    return graph
 
# a DFS based function to check if there is a cycle in the directed graph
def dfs_cycle(graph, node, onpath, visited):
    if visited[node]:
        return false
    onpath[node] = visited[node] = True
    for neigh in graph[node]:
        if (onpath[neigh] or dfs_cycle(graph, neigh, onpath, visited)):
            return true
    return False
 
def canFinish(numTasks, prerequisites):
    graph = make_graph(numTasks, prerequisites)
    onpath = [False]*numTasks
    visited = [False]*numTasks
    for i in range(numTasks):
        if (not visited[i] and dfs_cycle(graph, i, onpath, visited)):
            return False
    return True
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: O(E+V), where E is the number of dependencies and V is the number of tasks
<br>
Space Complexity: O(V), where V is the number of tasks