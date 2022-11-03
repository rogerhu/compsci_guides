## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/time-needed-to-inform-all-employees/](https://leetcode.com/problems/time-needed-to-inform-all-employees/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Breadth-First Search, Depth-First Search
* 🗒️ **Similar Questions**: [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/), [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can we transform the organization into a tree with each node containing the wait time (before transmitting down to their reports)?
    - This can simplify the problem into traversing the tree from the top node down to the bottom.
- How can we represent the company?
    - The company can be represented as a tree, headID is always the root. You can store for each node the time needed to be informed of the news. Answer is the max time a leaf node needs to be informed.
- How is the time calculated while traversing down and comparing and setting the maximum time?
- Can we avoid repeating the work of calculating how much time it takes for each manager to inform their superiors?
   
```markdown

HAPPY CASE
Input: n = 1, headID = 0, manager = [-1], informTime = [0]
Output: 0

Input: n = 4, headId = 2, manager = [3,3,-1,2], informTime = [0, 0, 162, 914]
Output: 1076

EDGE CASE
Input: n = 6, headID = 2, manager = [2,2,-1,2,2,2], informTime = [0,0,1,0,0,0]
Output: 1
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For graph problems, we want to consider the following approaches:

- Is this a directed or undirected graph? Directed graph. Direction from manager to employee or employee to manager?
- Is this BFS or DFS? Similar to root to leaf path. Can be done through DFS
- Adjacency List: We can use an adjacency list to store the graph, especially since the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort for the same reason we can use DFS, as in this problem, the application of DFS is a topological sort.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use DFS to traverse the graph while calculating time at each step

```markdown
1. We first convert the list to a graph
2. Then we use DFS to visit every note
3. We keep calculating the notifying time for each employee
4. we update the maximum time at each step
```

⚠️ **Common Mistakes**

* Some may not notice that there is a tree structure can be formed in this problem. The first thought that comes to mind when trying to solve this problem is that the deepest employee in the tree is going to require the most time until that become informed. We proceed down the hierarchy, layer by layer. The tree is a mapping from `employee_id` to list of subordinates.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution(object):
    def numOfMinutes(self, n, headID, managers, informTime):
        graph = self.buildGraph(managers)
        self.maxTime = 0
      
        def dfs(source, currentTime):
            # check if the source node is a non-manager
            self.maxTime = max(self.maxTime, currentTime)
            
            # explore the neighbors
            for neighbor in graph[source]:
                dfs(neighbor, currentTime + informTime[source])
        
        dfs(headID, 0)
        return self.maxTime
    
    # we will build a Directed Graph : Manager -> Employee
    def buildGraph(self, managers):
        # Adjacency List
        graph = defaultdict(list)
        for employee, manager in enumerate(managers):
            if manager != -1:
                graph[manager].append(employee)
            
        return graph
```
```java
public int numOfMinutes(int n, int headID, int[] manager, int[] informTime) {
    Map<Integer, List<Integer>> graph = new HashMap<>();
    for(int i = 0; i<manager.length; i++){
        if(manager[i] == -1) {
            continue;
        }
        graph.computeIfAbsent(manager[i], k-> new ArrayList()).add(i);
    }        
    return dfs(headID, 0, graph, informTime);
}
    
// apply dfs
public int dfs(int headId, int time, Map<Integer,List<Integer>> graph, int[] informTime){
    if(informTime[headId] == 0) {
        return time;
    }
    int count = informTime[headId];
    int val=Integer.MIN_VALUE;
    for(int childId : graph.get(headId)){
        val = Math.max(val, dfs(childId, time + count, graph, informTime));
    }
    return val;
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(n) since we traverse backward on each employee at most once
* **Space Complexity**: O(n) since we store the time it takes to inform each employee