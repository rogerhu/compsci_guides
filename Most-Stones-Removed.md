🔗 **Leetcode Link:** 

⏰ **Time to complete**: __ mins

1. **U-nderstand**

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
    
    For graph problems, some things we want to consider are:
    
    - We can possible use BFS to traverse the stones graph, but it will lead to a difficult solution.
        - We can use DFS to traverse the graph and use it to count the number of connected components.
    - We can use a map to store the edges in the graph to lookup rows by index to reduce average runtime complexity.
    - We can use union find to count the number of islands by adding each stone to union-find set, and counting number of sets.
3. P-lan
    
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
    
    ```java
    // Java Code
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
    # Python Code
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
    
    Verify the code works for the happy and edge cases you created in the “Understand” section
    
6. E-valuate
    - Time Complexity: O(N)
    - Space Complexity: O(N)