## Problem Highlights

* 🔗 **Leetcode Link:** [Number of People That Can Be Seen in a Grid](https://leetcode.com/problems/number-of-people-that-can-be-seen-in-a-grid/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Stack
* 🗒️ **Similar Questions**: [Number of Visible People in a Queue](https://leetcode.com/problems/number-of-visible-people-in-a-queue/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What happens when a newcomer encounters one with equal height? Can you provide an example?
  - When a newcomer encounters one with equal height, the newcomer replace the existing one and stops there. 
Eg. two persons in stack [height == 4, height == 2], a third one with height == 2 shows up, first person cannot see person 3, because his view of person 3 blocked by person 2.
- How is this different from [Number of Visible People in a Queue](https://leetcode.com/problems/number-of-visible-people-in-a-queue/)? 
  - One difference is that we can have duplicates here.

```markdown
Example 1
Input: heights = [[3,1,4,2,5]]
Output: [[2,1,2,1,0]]
Explanation:
- The person at (0, 0) can see the people at (0, 1) and (0, 2).
  Note that he cannot see the person at (0, 4) because the person at (0, 2) is taller than him.
- The person at (0, 1) can see the person at (0, 2).
- The person at (0, 2) can see the people at (0, 3) and (0, 4).
- The person at (0, 3) can see the person at (0, 4).
- The person at (0, 4) cannot see anybody.


Example 2
Input: heights = [[5,1],[3,1],[4,1]]
Output: [[3,1],[2,1],[1,0]]
Explanation:
- The person at (0, 0) can see the people at (0, 1), (1, 0) and (2, 0).
- The person at (0, 1) can see the person at (1, 1).
- The person at (1, 0) can see the people at (1, 1) and (2, 0).
- The person at (1, 1) can see the person at (2, 1).
- The person at (2, 0) can see the person at (2, 1).
- The person at (2, 1) cannot see anybody.
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Stack: What could we push onto a stack to make this problem easier? Stacks don’t allow us to keep track of data based on keys.
- Queue: Queues fall into the same category as Stacks, do we need to maintain any sense of ordering to solve this problem?
- HashMap: HashMaps allow us to store data for quick access. What could we store in a HashMap to make this problem easier?
- Heap: Do we need some sort of ordering to our data that a Heap could provide?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

```markdown
Maintain a stack and pop an element off and increment answer by 1 when the current height is more than the top element on the stack.
```

⚠️ **Common Mistakes**

* There is one thing to watch out for - when the current element is equal to the top element on the stack, do not push it onto the stack, just skip it. If we push onto the stack, it will lead to future bigger elements to over count.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def seePeople(self, heights: List[List[int]]) -> List[List[int]]:
        m, n = len(heights), len(heights[0])
        ans = [[0] * n for _ in range(m)]
        for i in range(m): 
            stack = []
            for j in range(n): 
                prev = -inf
                while stack and heights[i][stack[-1]] < heights[i][j]: 
                    if prev < heights[i][stack[-1]]: ans[i][stack[-1]] += 1
                    prev = heights[i][stack.pop()]
                if stack and prev < heights[i][stack[-1]]: ans[i][stack[-1]] += 1
                stack.append(j)
        for j in range(n): 
            stack = []
            for i in range(m): 
                prev = -inf 
                while stack and heights[stack[-1]][j] < heights[i][j]: 
                    if prev < heights[stack[-1]][j]: ans[stack[-1]][j] += 1
                    prev = heights[stack.pop()][j]
                if stack and prev < heights[stack[-1]][j]: ans[stack[-1]][j] += 1
                stack.append(i)
        return ans 
```

```java
class Solution {
    public int[][] seePeople(int[][] heights) {
        int m = heights.length, n = heights[0].length;
        int[][] ans = new int[m][n];
        for (int i = 0; i < n; i++){ // DOWN
            Deque<Integer> stack = new ArrayDeque<>();
            for (int j = m - 1; j >= 0; j--){
                while(!stack.isEmpty() && heights[j][i] > stack.peek()){
                    ans[j][i]++;
                    stack.pop();
                }
                if (!stack.isEmpty()){
                    ans[j][i]++;
                }
                if (stack.isEmpty() || heights[j][i] != stack.peek()){
                    stack.push(heights[j][i]);
                }
            }
        }

        for (int i = 0; i < m; i++){ // RIGHT
            Deque<Integer> stack = new ArrayDeque<>();
            for (int j = n - 1; j >= 0; j--){
                while(!stack.isEmpty() && heights[i][j] > stack.peek()){
                    ans[i][j]++;
                    stack.pop();
                }
                if (!stack.isEmpty()){
                    ans[i][j]++;
                }
                if (stack.isEmpty() || heights[i][j] != stack.peek()){
                    stack.push(heights[i][j]);
                }
            }
        }

        return ans;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(MN), where M and N are the sizes of its two inputs
* **Space Complexity**: O(N) since a stack is used