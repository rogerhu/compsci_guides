🔗 **Leetcode Link:** [https://leetcode.com/problems/number-of-islands](https://leetcode.com/problems/number-of-islands)

⏰ **Time to complete**: 15 mins

1. **U-nderstand**
    - What are the constraints?
        - `m` == grid.length (number of rows)
        - `n` == grid[i].length (number of columns)
        - 1 <= `m`, `n` <= 300
        - `grid[i][j]` is '0' or '1'
    - Can we simply iterate through the grid and count each time that there isn’t a 1 to the left or above another 1? 
    That won’t actually work though because there can be islands with isolated cells that have 0s above and to the left, but are still part of the island.
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
    
2. M-atch
    
    For this graph problem, some things we want to consider are:
    
    - Depth-First Search
        - The idea is to consider the given matrix as a graph, where each cell is a node of the given graph. Two nodes contain an edge if and only if there is a ‘1’ either horizontally or vertically.
    - Find-Union
        - Each set is a single island
        - Two pieces of land should be merged if they are adjacent
3. P-lan
    
    General Description of plan (1-2 sentences)
    
    1. Create the initial parent array
        1. Number of entries: nrows * ncols
        2. Set parent[i] = i for all elements
    2. For each element of the grid
        1. If this element is '1'
            1. If the left neighbor is '1', call union
            2. If the upper neighbor is '1', call union
    3. Return the number of islands (sets)
4. I-mplement


Java Solution:     
```java
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
    

Python Solution:
```python
    class Solution(object):
        def numIslands(self, grid):
            """
            :type grid: List[List[str]]
            :rtype: int
            """
            if len(grid) == 0: return 0
            row = len(grid); col = len(grid[0])
            self.count = sum(grid[i][j]=='1' for i in range(row) for j in range(col))
            parent = [i for i in range(row*col)]
            
            def find(x):
                if parent[x]!= x:
                    return find(parent[x])
                return parent[x]
            
            def union(x,y):
                xroot, yroot = find(x),find(y)
                if xroot == yroot: return 
                parent[xroot] = yroot
                self.count -= 1
            
            
            
            for i in range(row):
                for j in range(col):
                    if grid[i][j] == '0':
                        continue
                    index = i*col + j
                    if j < col-1 and grid[i][j+1] == '1':
                        union(index, index+1)
                    if i < row-1 and grid[i+1][j] == '1':
                        union(index, index+col)
            return self.count
```
    
5. R-eview
    
    Verify the code works for the happy and edge cases you created in the “Understand” section
    
6. E-valuate
    
    Calculate complexity in terms of:
    
    - the number of grid elements, `V`
    - the size of the maximum island, `I`
    
    Time complexity:
    
    - `O(V)` to build array
    - `O(V)` iterations
        - `O(log I)` for union operation
    - Total: **O(V log I)**
    - Can ensure `O(V log V)` by tuning union function
    
    Space complexity: **O(V)**