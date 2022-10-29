## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/number-of-islands](https://leetcode.com/problems/number-of-islands)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs, Breadth-First Search, Depth-First Search
* 🗒️ **Similar Questions**: [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/), [Walls and Gates](https://leetcode.com/problems/walls-and-gates/), [Number of Islands II](https://leetcode.com/problems/number-of-islands-ii/), [Number of Distinct Islands](https://leetcode.com/problems/number-of-distinct-islands/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What are the constraints?
   - `m` == grid.length (number of rows)
   - `n` == grid[i].length (number of columns)
   - 1 <= `m`, `n` <= 300
   - `grid[i][j]` is '0' or '1'

- Can we simply iterate through the grid and count each time that there isn’t a 1 to the left or above another 1? 
   - That won’t actually work though because there can be islands with isolated cells that have 0s above and to the left, but are still part of the island.

- What is the definition of an island?
   - One or more pieces of land ('1') connected horizontally or vertically
    

```markdown
    HAPPY CASE
    Input: [["1","1","1","1","0"],["1","1","0","1","0"],["1","1","0","0","0"],["0","0","0","0","0"]]
    Output: 1
    
    Input: [["1","1","0","0","0"],["1","1","0","0","0"],["0","0","1","0","0"],["0","0","0","1","1"]]
    Output: 3
    
    EDGE CASE
    Input: [["0","0","1","0","0"],["0","0","0","0","0"],["0","0","0","0","0"],["0","0","1","0","0"]]
    Output: 2
```
    
## 2: M-atch
    
> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

    For this graph problem, some things we want to consider are:
    
- DFS: The idea is to consider the given matrix as a graph, where each cell is a node of the given graph. Two nodes contain an edge if and only if there is a ‘1’ either horizontally or vertically.
- Union Find: Are there find and union operations here? Can you perform a find operation where you can determine which subset a particular element is in? This can be used for determining if two elements are in the same subset. Can you perform a union operation where you join two subsets into a single subset? Can you check if the two subsets belong to same set? If no, then we cannot perform union. We can imagine each set is a single island and two pieces of land should be merged if they are adjacent.

## 3: P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.
    
    1. Create the initial parent array
        - Number of entries: nrows * ncols
        - Set parent[i] = i for all elements
    2. For each element of the grid
        - If this element is '1'
            a. If the left neighbor is '1', call union
            b. If the upper neighbor is '1', call union
    3. Return the number of islands (sets)

⚠️ **Common Mistakes**

* When we are checking neighbors, we might end up filling queue with duplicate cells having '1' which will cause time limit exceeded error. To fix this problem, we can convert all 1s neighbors to 0s before adding them to the queue.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

     
```java
# Java Solution
    public int numIslands(char[][] grid) {
            if(grid == null || grid.length < 1 || grid[0].length < 1){
                return 0;
            }
            int row = grid.length;
            int col = grid[0].length;
            int[] root = new int[row * col];
            Arrays.fill(root, -1);
            int n = 0;
            for(int i = 0; i < row; i++){
                for(int j = 0; j < col; j++){
                    root[i*col + j] = i*col + j;
                    if(grid[i][j] == '1'){
                        n++;
                    }
                }
            }
            for(int i = 0; i < row; i++){
                for(int j = 0; j < col; j++){
                    if(grid[i][j] == '1'){
                        int p = i * col + j;
                        int q;
                        if((i+1) < row && grid[i+1][j] == '1'){
                            q = p + col;
                            n = union(root, p, q, n);
                        }
                        if((j+1) < col && grid[i][j+1] == '1'){
                            q = p + 1;
                            n = union(root, p, q, n);
                        }
                    }
                }
            }
            return n;
        }
        private int union(int[] root, int p, int q, int count){
            int proot = find(root, p);
            int qroot = find(root, q);
            if(proot!= qroot){
                root[proot] = qroot;
                count--;
            }
            return count;
        }
        private int find(int[] root, int p){
            while(p != root[p]){
                root[p] = root[root[p]];
                p = root[p];
            }
            return p;
        }
```
    


```python
def numIslands(self, grid: List[List[str]]) -> int:
	# keep track of row and column lengths of grid to access later
	m = len(grid)
	n = len(grid[0])

	def destroyIsland(r,c): # recursive DFS helper
		grid[r][c] = "2" # set this spot to a different value to avoid infinite loops
		for (row,col) in [(r-1,c),(r+1,c),(r,c-1),(r,c+1)]: # 4-dimensionally adjacent locations
			if 0 <= row < m and 0 <= col < n and grid[row][col] == "1": # if not out of bounds and part of the island
				destroyIsland(row,col) # destroy the rest of the island (set to "2")

	ans = 0 # answer
	for i, row in enumerate(grid): # get rows from grid
		for j, element in enumerate(row): # go through the row
			if element == "1": # if we hid land
				destroyIsland(i,j) # destroy this island
				ans += 1 # we found an island
	return ans
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errorS and verify the code works for the happy and edge cases you created in the “Understand” section
    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
Calculate complexity in terms of:
    
    - the number of grid elements, `V`
    - the size of the maximum island, `I`

Time complexity:
    
    - `O(V)` to build array
    - `O(V)` iterations
        - `O(log I)` for union operation
    - Total: **O(V log I)**
    - Can ensure `O(V log V)` by tuning union function
    
Space complexity: `O(V)`