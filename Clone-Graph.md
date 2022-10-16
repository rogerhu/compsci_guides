🔗 **Leetcode Link:** [https://leetcode.com/problems/clone-graph/](https://leetcode.com/problems/clone-graph/)

⏰ **Time to complete**: 10 mins

1. **U-nderstand**

- Does the graph have to a connected graph?
Yes, this graph has to be a connected graph. If it’s not connected, it’s impossible to clone a graph give only one node.
    
- What do we go after copying every single node?
We need to copy each single node and assign the correct reference to the copied node.
    
- Can a node in this graph have more than one neighbor?
Yes, a node could have any number of neighbors. This is why neighbors can be thought of as a list.
    
- How do we choose how to traverse the graph?
Based on the kind of graph we are expecting we can chose BFS or DFS. 
    
    ```markdown
    HAPPY CASE
    Input: adjList = [[]]
    Output: [[]]
    
    Input: adjList = []
    Output: []
    ```
    
2. M-atch
    
    For graph problems, some things we want to consider are:
    
    - DFS - We can use DFS to traverse all node of original graph. As soon as we reach a node, we will make a copy node. And recur for rest of the graph.
    - BFS - If the recursion stack is what we are worried about, then DFS is not the best solution. We use the BFS way of doing iterative traversal of the graph.

The difference is only in the traversal of DFS and BFS. DFS explores the depths of the graph first and BFS explores the breadth. 

1. P-lan
    
 
    1. Using DFS
        1. Check is current node is empty.
        2. Check if cached.
        3. Create a new node and save it into map.
        4. Use DFS to copy all its neighbors.
    2. Using BFS
        1. Use a hash map to store the reference of the copy of all the nodes that have already been visited and copied. 
        2. Add the first node to the queue. 
        3. Do the BFS traversal.
            - Pop a node from the front of the queue.
            - Visit all the neighbors of this node.
            - If any of the neighbors was already visited then it must be present in the `visited` dictionary. Get the clone of this neighbor from `visited` in that case.
            - Add the clones of the neighbors to the corresponding list of the clone node.
    
    **Common Mistakes**
    
    - Because you need to copy each single node and assign the correct reference to the copied node, you can easily make a mistake in assigning a pointer to the old reference.
    - To avoid cycles, we would need the `visited` hash map in both the BFS/DFS approaches. We need this to to keep track of the nodes which have already been copied. By doing this we don't end up traversing them again.
2. I-mplement
    
    ```java
    // DFS Java Code
    
    class Solution {
        private Map<Integer, Node> map;
        public Node cloneGraph(Node node) {
            map = new HashMap<>();
            return dfs(node, node.val);
        }
        private Node dfs(Node node, int num){
            if(node == null) return null;
            else if(map.containsKey(num)) return map.get(num);
            else{
                List<Node> neighbours = new ArrayList<>();
                Node cur = new Node(num, neighbours);
                map.put(num, cur);
                for(Node neighbour : node.neighbors){
                    neighbours.add(dfs(neighbour, neighbour.val));
                }
                return cur;
            }
        }
    }
    ```
    
    ```java
    // BFS Java Code
    
    public class Solution {
        public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
            if (node == null)
                return null;
            LinkedList<UndirectedGraphNode> q = new LinkedList<UndirectedGraphNode>();
            HashMap<Integer, UndirectedGraphNode> map = new HashMap<Integer, UndirectedGraphNode>();
            q.add(node);
            map.put(node.label, new UndirectedGraphNode(node.label));
            while (q.size() > 0){
                UndirectedGraphNode now = q.pollFirst();
                UndirectedGraphNode copied = map.get(now.label);
                copied.neighbors = new ArrayList<UndirectedGraphNode>();
                for (UndirectedGraphNode n:now.neighbors){
                    if (!map.containsKey(n.label)){
                        q.add(n);
                        map.put(n.label, new UndirectedGraphNode(n.label));
                    }
                    copied.neighbors.add(map.get(n.label));
                }
            }
            return map.get(node.label);
        }
    }
    ```
    
    ```python
    # DFS Python Code
    
    def cloneGraph(self, node):
        dict = {}
        stack = [node] if node else []
        while stack:
            i = stack.pop()
            if i not in dict:
                dict[i] = UndirectedGraphNode(i.label)
            stack.extend([j for j in i.neighbors if j not in dict])
        for i in dict:
            for j in i.neighbors:
                dict[i].neighbors.append(dict[j])
        return dict[node] if node else node
    ```
    
    ```python
    # BFS Python Code
    
    def cloneGraph(self, node):
        dict = {}
        stack = [node] if node else []
        for i in stack:
            if i not in dict:
                dict[i] = UndirectedGraphNode(i.label)
            stack.extend([j for j in i.neighbors if j not in dict])
        for i in dict:
            for j in i.neighbors:
                dict[i].neighbors.append(dict[j])
        return dict[node] if node else node
    ```
    
3. R-eview
    
    Verify the code works for the happy and edge cases you created in the “Understand” section
    
4. E-valuate
    - Time Complexity: *O*(*N*+*M*), where *N* is a number of nodes (vertices) and *M* is a number of edges
    - Space Complexity: *O*(*N*), accounting for the use of the hash map used in the Java solution or the stack used in the python solution