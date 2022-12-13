## Problem Highlights

* 🔗 **Leetcode Link:** [Clone Graph](https://leetcode.com/problems/maximal-network-rank)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Graph,Breadth-First Search, Depth-First Search
* 🗒️ **Similar Questions**: [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer), 
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Does the graph have to a connected graph?
  - Yes, this graph has to be a connected graph. If it’s not connected, it’s impossible to clone a graph give only one node.
    
- What do we go after copying every single node?
  - We need to copy each single node and assign the correct reference to the copied node.
    
- Can a node in this graph have more than one neighbor?
  - Yes, a node could have any number of neighbors. This is why neighbors can be thought of as a list.
    
- How do we choose how to traverse the graph?
  - Based on the kind of graph we are expecting, we can chose a BFS or DFS implementation. 
    

```markdown
HAPPY CASE
Input: adjList = [[2,4],[1,3],[2,4],[1,3]]
Output: [[2,4],[1,3],[2,4],[1,3]]
Explanation: There are 4 nodes in the graph.
1st node (val = 1)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
2nd node (val = 2)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).
3rd node (val = 3)'s neighbors are 2nd node (val = 2) and 4th node (val = 4).
4th node (val = 4)'s neighbors are 1st node (val = 1) and 3rd node (val = 3).

EDGE CASE
Input: adjList = [[]]
Output: [[]]
Explanation: Note that the input contains one empty list. The graph consists of only one node with val = 1 and it does not have any neighbors.

Input: adjList = []
Output: []
Explanation: This an empty graph, it does not have any nodes.
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Graph Problems, common solution patterns include:


- DFS: We can use DFS to traverse all node of original graph. As soon as we reach a node, we will make a copy node. And recur for rest of the graph.
- BFS: If the recursion stack is what we are worried about, then DFS is not the best solution. We use the BFS way of doing iterative traversal of the graph.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. We can use union find to count the number of islands by adding each stone to union-find set, and counting number of sets.
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.

Note: The difference is only in the traversal of DFS and BFS. DFS explores the depths of the graph first and BFS explores the breadth. 

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use BFS or DFS to mark visited nodes and to keep track of created copies

```markdown
DFS
0. Create visited dictionary to save created nodes in memory
1. Define dfs function:
    a. Check is current node is empty.
    b. Check if cached.
    c. Create a new node and save it into map.
    d. Use DFS to copy all its neighbors.
    e. Return deepcopy
2. Run dfs on given node
```
```markdown
BFS
1. Use a hash map to store the reference of the copy of all the nodes that have already been visited and copied. 
2. Add the first node to the queue. 
3. Do the BFS traversal.
    - Pop a node from the front of the queue.
    - Visit all the neighbors of this node.
    - If any of the neighbors was already visited then it must be present in the `visited` dictionary. Get the clone of this neighbor from `visited` in that case.
    - Add the clones of the neighbors to the corresponding list of the clone node.
```

⚠️ **Common Mistakes**


* Because you need to copy each single node and assign the correct reference to the copied node, you can easily make a mistake in assigning a pointer to the old reference.
* To avoid cycles, we would need the `visited` hash map in both the BFS/DFS approaches. We need this to to keep track of the nodes which have already been copied. By doing this we don't end up traversing them again.
 
## 4: I-mplement

> **Implement** the code to solve the algorithm.

**Approach #1: DFS**
```python
class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        # Create visited dictionary to save created nodes in memory
        visited = defaultdict(Node)
        
        def dfs(node: 'Node') -> 'Node':
            # Check is current node is empty.
            if not node:
                return None
            # Check if cached.
            elif node in visited:
                return visited[node]
            else:
                # Create a new node and save it into map
                deepcopy = Node(node.val)
                visited[node] = deepcopy
                # Use DFS to copy all its neighbors
                for neighbor in node.neighbors:
                    deepcopy.neighbors.append(dfs(neighbor))
            # Return deepcopy
            return deepcopy
        # Run dfs on given node
        return dfs(node)
```
```java
class Solution {
        // create a hash map to save the visited node and it's respective clone
        // as key and value respectively. This helps to avoid cycles.
        private Map<Integer, Node> map;
        public Node cloneGraph(Node node) {
            if (node == null) return null;
            map = new HashMap<>();
            return dfs(node, node.val);
        }

        private Node dfs(Node node, int num){
            if(node == null) return null;

            // if the node was already visited before, return the clone from the visited dictionary
            else if(map.containsKey(num)) return map.get(num);
            else{
                ArrayList<Node> neighbours = new ArrayList<Node>();
                Node cur = new Node(num, neighbours);
                map.put(num, cur);

                // iterate through the neighbors to generate their clones 
                // and prepare a list of cloned neighbors to be added to the cloned node.
                for(Node neighbour : node.neighbors){
                    neighbours.add(dfs(neighbour, neighbour.val));
                }
                return cur;
            }
        }
    }
```
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `V` represents the number of vertices.
Assume `E` represents the number of edges

* **Time Complexity**: O(V+E) We will need to visit each vertice and their edges
* **Space Complexity**: O(V) accounting for the use of a hash map to store all visited vertices. Also we are creating `V` nodes.



