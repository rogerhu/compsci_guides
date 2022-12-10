## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/spiral-matrix-iv/>
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 10 to 12 mins
* 🛠️ **Topics**: Linked Lists
* 🗒️ **Similar Questions**: [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- What does it mean to traverse spirally?
  - You can traverse spirally to get the required matrix: top-left to right, top-right to bottom, bottom-right to left, and bottom-left to top
- What do we store in the matrix?
  - We have to store the liked list's element in the matrix. 

Run through a set of example cases:

```markdown
HAPPY CASE
Input: m = 3, n = 5, head = [3,0,2,6,8,1,7,9,4,2,5,5,0]
Output: [[3,0,2,6,8],[5,0,-1,-1,1],[5,2,4,9,7]]

Input: m = 1, n = 4, head = [0,1,2]
Output: [[0,1,2,-1]]

EDGE CASE
Input: m = 1, n = 1, head = [0]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Multiple passes: To find the length, or save other information about the contents. If we were able to take multiple passes of the linked list, would that help solve the problem? Multiple passes would not help this problem since we aren’t collecting unique items or need any pre-processing.
- Two pointers: ‘Race car’ strategy with one regular pointer, and one fast pointer. If we used two pointers to iterate through the list at different speeds, would that help us solve this problem?
A slow and fast pointer may not help the problem.
- Dummy node: Helpful for preventing errors when returning ‘head’ if merging lists, deleting from lists. Would using a dummy head as a starting point help simplify our code and handle edge cases? Since we have to manipulate the order of the list, including the head of the list, a dummy head would help maintain a pointer to beginning of the list.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Initialize the answer set with all elements as -1. If end of link list is reached, return the answer set else update the current index value to node's current value

```markdown
1. Try to go up from current index
        if you can go left as well from current index, go left
            add the left index to visited and call method again
            from left cell
        else go up
            add the up index to visited
            call method again from upward cell
            
2. Try to go right from current index
   if possible, add the index to visited and call method again
   from the right cell
   
3. Try to go bottom from current index
   if possible, add the index to visited and call method again
   from bottom cell
   
4. Try to go left from current index
   if possible, add the index to visited and call method again
   from left cell
```

⚠️ **Common Mistakes**

* The tricky part is that when you traverse left or up, you have to check whether the row or column still exists to prevent duplicates. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def spiralMatrix(self, m: int, n: int, head: Optional[ListNode]) -> List[List[int]]:
        ans = [[-1 for _ in range(n)] for _ in range(m)]
        visited = set()
        
        def is_valid(i, j):
            if 0 <= i < m and 0 <= j < n:
                return True
            return False
        
        def fun(i, j, head):
            if head is None:
                return ans
            ans[i][j] = head.val
            
            if is_valid(i-1, j) and (i-1, j) not in visited:
                if is_valid(i, j-1) and(i, j-1) not in visited:
                    visited.add((i, j-1))
                    return fun(i, j-1, head.next)
                else:
                    visited.add((i-1, j))
                    return fun(i-1, j, head.next)
                    
            if is_valid(i, j+1) and (i, j+1) not in visited:
                visited.add((i,j+1))
                return fun(i, j+1, head.next)
            
            if is_valid(i+1,j) and (i+1, j) not in visited:
                visited.add((i+1, j))
                return fun(i+1, j, head.next)
            
            if is_valid(i, j-1) and (i, j-1) not in visited:
                visited.add((i, j-1))
                return fun(i, j-1, head.next)
            
            return ans
            
        visited.add((0,0))
        return fun(0, 0, head)
```
```java
class Solution {
    
    // Add a instance variable to have helper method to fetch value.
    ListNode node;
    
    public int[][] spiralMatrix(int m, int n, ListNode head) {
        node = head; // point to the head
        
        // new array
        int [][]A = new int[m][n];
        
        // define boundaries for spiral
        int left = 0, top = 0;
        int right = n-1, bottom = m-1;
        
        // variable to keep track of boxes filled
        int count = 0, maxCount = m * n;
        
        while (true) {
            
            // left to right
            for (int i = left; i <= right; i++) {
                A[top][i] = getNext();
                count++;
            }
            top++; // top moved down, as we have filled the top while progressing left to right.
            if (count >= maxCount) break; // if all boxes filled, we are done.
            
            
            // top to bottom
            for (int j = top; j <= bottom; j++) {
                A[j][right] = getNext();
                count++;
            }
            right--; // right shrinks towards left, as we filled the current right.
            if (count >= maxCount) break; 
            
            
            // right to left
            for (int k = right; k >= left; k--) {
                A[bottom][k] = getNext();
                count++;
            }
            bottom--; // bottom moves up, as we filled the bottom row.
            if (count >= maxCount) break; // if all boxes filled, we are done.
            
            
            // bottom to top
            for (int l = bottom; l >= top; l--) {
                A[l][left] = getNext();
                count++;
            }
            left++; // left shrinks towards right, we filled the current left.
            if (count >= maxCount) break; // if all boxes filled, we are done.
        }
        
        return A;
    }
    
    // helper method keep returning elements from linkedList, once list is exhausted, it starts returning -1.
    int getNext() {
        if (node == null) return -1;
        
        int val = node.val;
        node = node.next;
        return val;
    }
}   
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(n), where n is the length of the linked list
* **Space Complexity**: O(1)
