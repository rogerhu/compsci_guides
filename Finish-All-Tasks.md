## Problem Highlights

🔗 **Leetcode Link:** [https://www.geeksforgeeks.org/find-whether-it-is-possible-to-finish-all-tasks-or-not-from-given-dependencies](https://www.geeksforgeeks.org/find-whether-it-is-possible-to-finish-all-tasks-or-not-from-given-dependencies)

⏰ **Time to complete**: __ mins

## 1. **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How do we encode the prerequisites into a graph?
You can denote each course as a node and the prerequisite relationship as an one-direction edge.

- When is it impossible for you to finish all courses?
When there exists at least one task pair t1 and t2, such that t1 is direct or indirect prerequisite for t2 and t2 is direct or indirect prerequisite for t1. This is equivalent to finding a cycle in our graph representation.
    
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
    
## 2. M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    How can we apply DFS on this problem?
    Given a starting vertex, it’s wise to find all vertices reachable from the start. There are many algorithms to do this, the simplest is the use of depth-first search. DFS enumerates the deepest paths. DFS only backtracks when it hits a dead end or an already-visited section of the graph.
    
## 3. P-lan

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
    
    **Common Mistakes:**
    
    - Each node is visited more than once.
    - Using Kahn's algorithm, you can peel off the nodes with indegree 0, rather than nodes with outdegree 0.

## 4. I-mplement

> **Implement** the code to solve the algorithm.
    
```java
    public class CanFinish {
    
        List<List<Integer>> edges;
        int[] indeg;
    
        public boolean canFinish(int numCourses, int[][] prerequisites) {
            edges = new ArrayList<>();
            for (int i = 0; i < numCourses; ++i) {
                edges.add(new ArrayList<>());
            }
            indeg = new int[numCourses];
            for (int[] info : prerequisites) {
                edges.get(info[1]).add(info[0]);
                ++indeg[info[0]];
            }
    
            Queue<Integer> queue = new LinkedList<>();
            for (int i = 0; i < numCourses; ++i) {
                if (indeg[i] == 0) {
                    queue.offer(i);
                }
            }
    
            int visited = 0;
            while (!queue.isEmpty()) {
                ++visited;
                int u = queue.poll();
                for (int v: edges.get(u)) {
                    --indeg[v];
                    if (indeg[v] == 0) {
                        queue.offer(v);
                    }
                }
            }
    
            return visited == numCourses;
        }
  }
```
    
```python
    def canFinish(self, n, prerequisites):
        arr = [[] for i in range(n)]
        degree = [0] * n
        for j, i in prerequisites:
            arr[i].append(j)
            degree[j] += 1
        dfs = [i for i in range(n) if degree[i] == 0]
        for i in dfs:
            for j in arr[i]:
                degree[j] -= 1
                if degree[j] == 0:
                    dfs.append(j)
        return len(dfs) == n
    
     -> List[int]:
            numNodes = len(graph)
            state = [Solution.UNVISITED] * numNodes
            for node in range(numNodes):
                if state[node] == Solution.UNVISITED:
                    self.dfs(graph, node, state)
            safeNodes = []
            for (i, nodeState) in enumerate(state):
                if nodeState == Solution.SAFE:
                    safeNodes.append(i)
            return safeNodes
        
        def dfs(self, graph, currNode, state):
            if state[currNode] == Solution.SAFE:
                return True
            if state[currNode] == Solution.VISITING or state[currNode] == Solution.UNSAFE:
                return False
            state[currNode] = Solution.VISITING
            isSafe = True
            for neighbor in graph[currNode]:
                isSafe &= self.dfs(graph, neighbor, state)
                if not isSafe:
                    break
            state[currNode] = Solution.SAFE if isSafe else Solution.UNSAFE
            return isSafe
```
    
## 5. R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errorS and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6. E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: O(E+V)
<br>
Space Complexity: O(V)