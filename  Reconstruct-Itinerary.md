🔗 **Leetcode Link:** [https://leetcode.com/problems/reconstruct-itinerary/](https://leetcode.com/problems/reconstruct-itinerary/)

⏰ **Time to complete**: __ mins

1. **U-nderstand**

- Are all ticket names the same length?
Yes, they are all of three characters.

- What are the valid characters for the ticket names?
They are all uppercase alphabet characters.

- Can a ticket have the same starting and ending location?
No, tickets cannot start and end at the same place.
    
    ```markdown
    Input: tickets = [["MUC","LHR"],["JFK","MUC"],
     ["SFO","SJC"],["LHR","SFO"]]
    Output: ["JFK","MUC","LHR","SFO","SJC"]
    
    Input: tickets = [["JFK","SFO"],["JFK","ATL"],
     ["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]
    Output: ["JFK","ATL","JFK","SFO","ATL","SFO"]
    Explanation: Another possible reconstruction is 
     ["JFK","SFO","ATL","JFK","ATL","SFO"] 
     but it is larger in lexical order.
    ```
    
2. M-atch
    
    For graphs, some of the top things we want to consider are:
    
    - We cannot use BFS to traverse the graph because we may visit exit nodes in the first traversal.
    - We can use DFS to traverse the graph until we find an valid itinerary, ensuring we choose the lexicographically least itinerary.
    - We can use an adjacency list to store graph, especially since the graph is sparse.
    - We can use an adjacency matrix to store graph, but a sparse graph will cause an unneeded worst case runtime.
    - We can use topological sort for the same reason we can use DFS, as in this problem, the application of DFS is a topological sort.
3. P-lan
    
    ```
    1. Sort the tickets
    2. Build the graph, assuring all adjacent nodes are in sorted order
    3. Create current path array to keep track of path
    4. Traverse in DFS fashion from "JFK":
       a. From current node, visit all adjacent nodes
          i.  Remove that ticket (edge) from the graph
          ii. Traverse that airport (node) in DFS
       b. Add current node to current path
    5. Return reverse of current path array
    ```
    
4. I-mplement
    
    ```java
    // Java Code
    public List<String> findItinerary(List<List<String>> tickets) {
       HashMap<String, PriorityQueue<String>> ticketGraph = new HashMap<>();
       for (List<String> ticket : tickets) {
          PriorityQueue<String> nextCities = ticketGraph.get(ticket.get(0));
          if (nextCities == null) {
             nextCities = new PriorityQueue<>();
             ticketGraph.put(ticket.get(0), nextCities);
          }
          nextCities.add(ticket.get(1));
       }
       LinkedList<String> route = new LinkedList<>();
       visit("JFK", ticketGraph, route);
       Collections.reverse(route);
       return route;
    }
    
    public void visit(String airport, 
                      HashMap<String, PriorityQueue<String>> ticketGraph, 
                      LinkedList<String> route) {
       PriorityQueue<String> nextCities = ticketGraph.get(airport);
       while (nextCities != null && !nextCities.isEmpty()) {
          String nextCity = nextCities.poll();
          visit(nextCity, ticketGraph, route);
       }
       route.add(airport);
    }
    ```
    
    ```python
    # Python Code
    def find_itinerary(tickets):
        ticket_graph = defaultdict(list)
        sorted_tickets = sorted(tickets)
        for start, end in sorted_tickets:
            ticket_graph[start].append(end)
        route = []
        visit('JFK', ticket_graph, route)
        route.reverse()
        return route
    
    def visit(airport, ticket_graph, route):
        while ticket_graph[airport]:
            next_city = ticket_graph[airport].pop(0)
            visit(next_city, ticket_graph, route)
        route.append(airport)
    ```
    
5. R-eview
    
    Verify the code works for the happy and edge cases you created in the “Understand” section
    
6. E-valuate
    - Time Complexity: O(N*logN)
    - Space Complexity: O(N)