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

- What is the good path?
  - The graph is represented by an array of values and a 2D array of edges. A good path is defined as a path in the graph where the maximum value of the vertices in the path is minimized.
- How many nodes and edges are there?
  - There are N nodes and N-1 edges, which means that there exists a unique path between each pair of nodes.


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
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? The approach used in this code is to sort the edges based on the maximum value of the vertices in the edge, and then use a union-find algorithm to group the vertices in the graph into connected components. The union-find algorithm is implemented using a disjoint set data structure, which is a collection of disjoint sets (or connected components) in the graph.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

*General idea: use a union-find algorithm to group the vertices in the graph into connected components. Sort the edges based on the maximum value of the vertices in the edge to minimize the maximum value of the vertices in a good path. And return the number of good paths in the graph.


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

* Realize BFS too slow
* Noticed that there exists a unique path between any 2 nodes in a tree
* Notice that the only way it can run in time is to have linear time, a.k.a solution need to go through nodes and edges in linear time
* Note that you want to sort the node because cheaper nodes can build up to the bigger node
* The trick that does the magic here is that we focus on the edges instead of the vertices, we prioritize the edges that connect the node values (i.e compare the vertices of the edges, and prioritize the on that has the minimum max of the pair). So when we are merging the groups we know that the edge that is merging them is the one with the minimum maximum possible. Build the graph step by step from edges connecting nodes with smaller values to those connecting larger valued nodes. Every time, try to figure out wether the current largest valued pairs of nodes in the graph are connected.

 
## 4: I-mplement

> **Implement** the code to solve the algorithm.


```python
class Union_Set:
    def __init__(self,n):
        self.parents = [i for i in range(n)]
        self.rank = [0 for i in range(n)]
        
    def find(self,x):
        if self.parents[x] != x:
            self.parents[x] = self.find(self.parents[x])
        return self.parents[x]
    
    def union(self,x,y):
        xp, yp = self.find(x), self.find(y)
        
        if xp == yp:
            return 
        elif self.rank[xp] == self.rank[yp]:
            self.parents[xp] = yp
            self.rank[yp] += 1
        elif self.rank[xp] < self.rank[yp]:
            self.parents[xp] = yp
        else:
            self.parents[yp] = xp


class Solution:
    def numberOfGoodPaths(self, vals: List[int], edges: List[List[int]]) -> int:
        tree = defaultdict(lambda:set())
        sorted_values = defaultdict(list)
        keys = set()
        for i in range(len(vals)):
            keys.add(vals[i])
            sorted_values[vals[i]].append(i)
        
        for edge in edges:
            tree[edge[0]].add(edge[1])
            tree[edge[1]].add(edge[0])
        skeys = sorted(list(keys))
        components = Union_Set(len(vals))
        
        res = 0
        for k in skeys:
            for node in sorted_values[k]:
                for neighbour in tree[node]:
                    if vals[node] >= vals[neighbour]:
                        components.union(neighbour,node)
            baby_graph = defaultdict(int)
            for node in sorted_values[k]:
                baby_graph[components.find(node)]+= 1
            for baby,nums in baby_graph.items():
                res += nums*(nums+1)/2
        return int(res)
```

```java
class UnionFind {
    int[] parent;
    int[] rank;

    public UnionFind(int size) {
        parent = new int[size];
        for (int i = 0; i < size; i++)
            parent[i] = i;
        rank = new int[size];
    }

    public int find(int x) {
        if (parent[x] != x)
            parent[x] = find(parent[x]);
        return parent[x];
    }

    public void union_set(int x, int y) {
        int xset = find(x), yset = find(y);
        if (xset == yset) {
            return;
        } else if (rank[xset] < rank[yset]) {
            parent[xset] = yset;
        } else if (rank[xset] > rank[yset]) {
            parent[yset] = xset;
        } else {
            parent[yset] = xset;
            rank[xset]++;
        }
    }
}

class Solution {
    public int numberOfGoodPaths(int[] vals, int[][] edges) {
        Map<Integer, List<Integer>> adj = new HashMap<>();
        for (int[] edge : edges) {
            int a = edge[0], b = edge[1];
            adj.computeIfAbsent(a, value -> new ArrayList<Integer>()).add(b);
            adj.computeIfAbsent(b, value -> new ArrayList<Integer>()).add(a);
        }

        int n = vals.length;
        // Mapping from value to all the nodes having the same value in sorted order of
        // values.
        TreeMap<Integer, List<Integer>> valuesToNodes = new TreeMap<>();
        for (int i = 0; i < n; i++) {
            valuesToNodes.computeIfAbsent(vals[i], value -> new ArrayList<Integer>()).add(i);
        }

        UnionFind dsu = new UnionFind(n);
        int goodPaths = 0;

        // Iterate over all the nodes with the same value in sorted order, starting from
        // the lowest value.
        for (int value : valuesToNodes.keySet()) {
            // For every node in nodes, combine the sets of the node and its neighbors into
            // one set.
            for (int node : valuesToNodes.get(value)) {
                if (!adj.containsKey(node))
                    continue;
                for (int neighbor : adj.get(node)) {
                    // Only choose neighbors with a smaller value, as there is no point in
                    // traversing to other neighbors.
                    if (vals[node] >= vals[neighbor]) {
                        dsu.union_set(node, neighbor);
                    }
                }
            }
            // Map to compute the number of nodes under observation (with the same values)
            // in each of the sets.
            Map<Integer, Integer> group = new HashMap<>();
            // Iterate over all the nodes. Get the set of each node and increase the count
            // of the set by 1.
            for (int u : valuesToNodes.get(value)) {
                group.put(dsu.find(u), group.getOrDefault(dsu.find(u), 0) + 1);
            }
            // For each set of "size", add size * (size + 1) / 2 to the number of goodPaths.
            for (int key : group.keySet()) {
                int size = group.get(key);
                goodPaths += size * (size + 1) / 2;
            }
        }
        return goodPaths;
    }
}
```


## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Given n is the number of nodes, 

* **Time Complexity**: O(n logn)
* **Space Complexity**: O(n)



