## Problem Highlights

* 🔗 **Leetcode Link:** [The Time When the Network Becomes Idle](https://leetcode.com/problems/the-time-when-the-network-becomes-idle/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Graph 
* 🗒️ **Similar Questions**: [Network Delay Time](https://leetcode.com/problems/network-delay-time/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Are there duplicate edges?
  - No, there are no duplicate edges. 
- Can a server reach another server?
  - Each server can directly or indirectly reach another server.

```markdown
Input: edges = [[0,1],[1,2]], patience = [0,2,1]
Output: 8
Explanation:
At (the beginning of) second 0,
- Data server 1 sends its message (denoted 1A) to the master server.
- Data server 2 sends its message (denoted 2A) to the master server.

At second 1,
- Message 1A arrives at the master server. Master server processes message 1A instantly and sends a reply 1A back.
- Server 1 has not received any reply. 1 second (1 < patience[1] = 2) elapsed since this server has sent the message, therefore it does not resend the message.
- Server 2 has not received any reply. 1 second (1 == patience[2] = 1) elapsed since this server has sent the message, therefore it resends the message (denoted 2B).

At second 2,
- The reply 1A arrives at server 1. No more resending will occur from server 1.
- Message 2A arrives at the master server. Master server processes message 2A instantly and sends a reply 2A back.
- Server 2 resends the message (denoted 2C).
...
At second 4,
- The reply 2A arrives at server 2. No more resending will occur from server 2.
...
At second 7, reply 2D arrives at server 2.

Starting from the beginning of the second 8, there are no messages passing between servers or arriving at servers.
This is the time when the network becomes idle.
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Graph Problems, common solution patterns include:


- DFS/BFS: We could use either BFS or DFS. DFS is fewer lines of code, but BFS makes use of the adjacency dictionary data structure.
- Adjacency List: We already have an adjacency list, let's make it a adjacency dictionary.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but this will make the problem more complicated.
- Topological Sort: In order to have a topological sorting, the graph must not contain any cycles. We cannot apply this sort to this problem because we can have cycles in our graph.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.


```markdown
1) Build an adjency list.
2) BFS from the master node to get the shortest path from node to master.
3) Calculate when the last resent time is, and how long it will take to reach the server after that.
```


⚠️ **Common Mistakes**

* The main idea is to find the last time any node receives a message. Don't forget a trip back to data server takes 2 * distance time.

## 4: I-mplement

> **Implement** the code to solve the algorithm.


```python
class Solution:
    def networkBecomesIdle(self, edges: List[List[int]], patience: List[int]) -> int:

        graph = defaultdict(list)
        for u,v in edges:
            graph[u].append(v)
            graph[v].append(u)

        time_tracker = [float("inf")] * len(patience)
        time_tracker[0] = 0

        heap = []
        heappush(heap,(0,0))

        while heap:
            time,node = heappop(heap)
            for nei in graph[node]:
                if time+1 < time_tracker[nei]:
                    heappush(heap,(time+1, nei))
                    time_tracker[nei] = time+1

        max_time_needed = 0
        for i in range(1, len(time_tracker)):
            
            time_needed = 2*time_tracker[i]
            
            msgs_sent = ceil(time_needed/patience[i])
           
            total_time_needed = (patience[i]) * (msgs_sent-1) + time_needed
            max_time_needed = max(max_time_needed, total_time_needed)

        return max_time_needed + 1
               
```

```java
public int networkBecomesIdle(int[][] edges, int[] patience) {

    int n = patience.length;
    ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
    boolean[] visited = new boolean[n + 1];
    int[] minDis = new int[n + 1];
    for (int i = 0; i <= n; i++) {
        adj.add(new ArrayList<>());
    }

    int m = edges.length;
    for (int i = 0; i < m; i++) {
        adj.get(edges[i][0]).add(edges[i][1]);
        adj.get(edges[i][1]).add(edges[i][0]);
    }
    Queue<int[]> q = new LinkedList<>();
    q.add(new int[]{0, 0});
    visited[0] = true;
    while (!q.isEmpty()) {
        int[] temp = q.poll();
        for (int i = 0; i < adj.get(temp[0]).size(); i++) {
            int u = adj.get(temp[0]).get(i);
            if (!visited[u]) {
                visited[u] = true;
                minDis[u] = temp[1] + 1;
                q.add(new int[]{u, minDis[u]});
            }
        }
    }

    int ans = 0;
    for (int i = 1; i < n; i++) {

        int time = 2 * minDis[i];
        ans = Math.max(time, ans);
        int num = time / patience[i];
        if (time % patience[i] == 0) {
            num--;
        }
        int lastMessage = num * patience[i] + 2 * minDis[i];
        ans = Math.max(lastMessage, ans);

    }
    return ans+1;
}
```


## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `V` represents the number of vertices/nodes and
Assume `E` represents the number of edges

* **Time Complexity**: O(ElogV) where, V is the number of vertices and E is the total number of edges
* **Space Complexity**: O(V) for the graph and time_tracker array



