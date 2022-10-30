## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/reconstruct-itinerary/](https://leetcode.com/problems/reconstruct-itinerary/)
* 💡 **Problem Difficulty:** Hard
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Depth-First Search, Adjacency List
* 🗒️ **Similar Questions**: [Longest Common Subpath](https://leetcode.com/problems/longest-common-subpath/), [Valid Arrangement of Pairs](https://leetcode.com/problems/valid-arrangement-of-pairs/)
    
## 1: U-nderstand

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Are all ticket names the same length?
  - Yes, they are all three characters.
- What are the valid characters for the ticket names?
   - They are all uppercase alphabet characters.
- Can a ticket have the same starting and ending location?
   - No, tickets cannot start and end at the same place.
   
```markdown
HAPPY CASE
Input: tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
Output: ["JFK","MUC","LHR","SFO","SJC"]
    
EDGE CASE
Input: tickets = [["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g., Linked List or Dynamic Programming, and strategies or patterns in those categories.

For graphs, some of the top things we want to consider are:
        
- BFS: We cannot use BFS to traverse the graph because we may visit exit nodes in the first traversal.
- DFS: We can use DFS to traverse the graph until we find a valid itinerary, ensuring we choose the lexicographically least itinerary.
- Adjacency List: We can use an adjacency list to store the graph, especially when the graph is sparse.
- Adjacency Matrix: We can use an adjacency matrix to store the graph, but a sparse graph will cause an unneeded worst-case runtime.
- Topological Sort: We can use topological sort when a directed graph is used and returns an array of the nodes where each node appears before all the nodes it points to. In order to have a topological sorting, the graph must not contain any cycles.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. 


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Build an adjacency list with sorted tickets and keep track of the current path as we traverse using DFS.

```markdown
1. Sort the tickets
2. Build the graph, assuring all adjacent nodes are in sorted order
3. Create a current path array to keep track of the path
4. Traverse in DFS fashion from "JFK":
   a. From the current node, visit all adjacent nodes
      i.  Remove that ticket (edge) from the graph
      ii. Traverse that airport (node) in DFS
   b. Add the current node to the current path
5. Return the reverse of the current path array
```

⚠️ **Common Mistakes**

* When setting up the queue, consider having two tickets with same depatures. Don't fall into the trap of creating priority queue for all departure as it can be duplicated.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution(object):
    def findItinerary(self, tickets):
        result=[]
        # create a graph
        graph = collections.defaultdict(list)
        for frm, to in tickets: 
            # rearrange the destination of same start together    
            graph[frm].append(to)   
        # get key and value from dictionary, sort the destination                
        for frm, tos in graph.items():      
            tos.sort(reverse=True)     
        
        def dfs(graph, source, result):
            while graph[source]:   
                # let the destination empty if we choose it to pop           
                new_source = graph[source].pop()         
                dfs(graph, new_source, result)
            result.append(source)
            
        dfs(graph, "JFK", result)         
        return result[::-1]
```
```java
class Solution {
    public List<String> findItinerary(List<List<String>> tickets) {
        HashMap<String , PriorityQueue<String>> map = new HashMap<>();
        // map the ticket from and to values
        for(List<String> ticket: tickets){
            map.putIfAbsent(ticket.get(0),new PriorityQueue<String>());
            map.get(ticket.get(0)).add(ticket.get(1));
        }
        LinkedList<String> ans = new LinkedList<>();
        // call the dfs method on JFK as that is the starting point
        dfs(map, ans, "JFK");
        return ans;
    }
    public static void dfs(HashMap<String , PriorityQueue<String>> map, LinkedList<String> ans, String search){
        // get the priority queue of our search from value
        PriorityQueue<String> currentqueue= map.get(search);
        // iterate all the to places one by one
        while(currentqueue != null && !currentqueue.isEmpty() ){
            // remove the place as we have counted(that ticket) them in
            String newSearch = map.get(search).poll();
            // cal dfs on the new Search
            dfs(map,ans,newSearch);
        }
        // add at the starting of the list
        ans.addFirst(search);
        return;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of tickets.

Time Complexity: `O(N*logN)`
<br>
Space Complexity: `O(N)`
