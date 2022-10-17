🔗 **Leetcode Link:** 

⏰ **Time to complete**: __ mins

1. **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How many stones can there be on the plane?
There can be up to 1000 stones.

- Could there be no stones?
Yes, there could be no stones.

- What if no stones can be removed?
Then the result should be zero.
    
    ```markdown
    HAPPY CASE
    Input: stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
    Output: 5
    
    EDGE CASE
    Input: stones = [[0,0]]
    Output: 0
    ```
    
2. M-atch
    
> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

    For graph problems, some things we want to consider are:
    
    - We can possible use BFS to traverse the stones graph, but it will lead to a difficult solution.
        - We can use DFS to traverse the graph and use it to count the number of connected components.
    - We can use a map to store the edges in the graph to lookup rows by index to reduce average runtime complexity.
    - We can use union find to count the number of islands by adding each stone to union-find set, and counting number of sets.

3. P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.

    We can first create an empty disjoint set, add each stone in a loop, and at each iteration compute the number of sets. At the end, # of stones - # of sets.
    
    `1) Create a disjoint set. 2) For each stone
      a) Add the coordinate x, ~y (i.e. -(y-1)) to the set
      b) Update number of sets
    3) Return number of stones - number of sets
    
    Time Complexity: O(N)
    Space Complexity: O(N)`
    
    **Common Mistakes:**
    
    - Some people may want to sort the array first, but there are faster solutions if we avoid sorting. We can reduce runtime complexity from O(N*log(N)) to O(N+M) if we avoid sorting either of the arrays. Though, we can sort for O(1) space complexity.

4. I-mplement

> **Implement** the code to solve the algorithm.
    
    ```java
    def removeStones(stones):
      f = {}
      islands = 0
      def find(x):
        nonlocal islands
        if x not in f:
          f[x] = x
          islands += 1
        if x != f[x]:
          f[x] = find(f[x])
        return f[x]
    
      def union(x, y):
        nonlocal islands
        x = find(x)
        y = find(y)
        if x != y:
          f[x] = y
          islands -= 1
    
      for [x, y] in stones:
        union(x, ~y)
      return len(stones) - islands
    ```
    
    ```python
    public class Solution {
      Map<Integer, Integer> f = new HashMap<>();
      int islands = 0;
    
      public int removeStones(int[][] stones) {
          for (int i = 0; i < stones.length; ++i)
              union(stones[i][0], -stones[i][1]);
          return stones.length - islands;
      }
    
      public int find(int x) {
          if (f.putIfAbsent(x, x) == null)
              islands++;
          if (x != f.get(x))
              f.put(x, find(f.get(x)));
          return f.get(x);
      }
    
      public void union(int x, int y) {
          x = find(x);
          y = find(y);
          if (x != y) {
              f.put(x, y);
              islands--;
          }
      }
    }
    ```
    
5. R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errorS and verify the code works for the happy and edge cases you created in the “Understand” section

    
6. E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: O(N)
<br>
Space Complexity: O(N)