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
    
    How can we apply DFS on this problem?
    Given a starting vertex, it’s wise to find all vertices reachable from the start. There are many algorithms to do this, the simplest is the use of depth-first search. DFS enumerates the deepest paths. DFS only backtracks when it hits a dead end or an already-visited section of the graph.
    
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.
    
    ```
    1) if(current is already processed), return false
    2) if(current is in processing state), return true (cycle is found)
    3) reaching means current is unprocessed, so mark current as processing
    4) process all neighbors of current if during processing any cycle is found for neighbor
    5) else, push neighbor to res
    6) then, mark current as processed
    
    Time Complexity: O(|E|+|V|)
    Space Complexity: O(V)
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