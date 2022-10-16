🔗 **Leetcode Link:** [https://www.geeksforgeeks.org/find-whether-it-is-possible-to-finish-all-tasks-or-not-from-given-dependencies](https://www.geeksforgeeks.org/find-whether-it-is-possible-to-finish-all-tasks-or-not-from-given-dependencies)

⏰ **Time to complete**: 15 mins

1. **U-nderstand**

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
    
2. M-atch
    
    How can we apply DFS on this problem?
    Given a starting vertex, it’s wise to find all vertices reachable from the start. There are many algorithms to do this, the simplest is the use of depth-first search. DFS enumerates the deepest paths. DFS only backtracks when it hits a dead end or an already-visited section of the graph.
    
3. P-lan
    
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

4. I-mplement
    
    ```java
    import java.util.ArrayList;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Queue;
    
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
    
        public static void main(String[] args) {
            CanFinish canFinish = new CanFinish();
            System.out.println(!canFinish.canFinish(4, new int[][]{{0,1},{3,1},{1,3},{3,2}}));
            System.out.println(canFinish.canFinish(5, new int[][]{{1,4},{2,4},{3,1},{3,2}}));
            System.out.println(!canFinish.canFinish(3, new int[][]{{1,0},{2,0},{0,2}}));
            System.out.println(!canFinish.canFinish(2, new int[][]{{1, 0},{0,1}}));
            System.out.println(!canFinish.canFinish(13, new int[][]{{1,2},{2,3},{2,10},{3,4},{4,5},{4,11},{5,1}}));
            System.out.println(!canFinish.canFinish(3, new int[][]{{1,0},{0,2},{2,1}}));
    
            System.out.println(canFinish.canFinish(8, new int[][]{{1,0},{2,6},{1,7},{6,4},{7,0},{0,5}}));
            System.out.println(!canFinish.canFinish(20, new int[][]{{0,10},{3,18},{5,5},{6,11},{11,14},{13,1},{15,1},{17,4}}));
    
            System.out.println(canFinish.canFinish(2, new int[][]{{1, 0}}));
        }
    }
    ```
    
    ```python
    # Python Code
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
    
5. R-eview
    
    Verify the code works for the happy and edge cases you created in the “Understand” section
    
6. E-valuate
    - Time Complexity: O(E+V)
    - Space Complexity: O(V)