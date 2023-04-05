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

- 

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

**General Idea:** When pushing a new value to the stack, check if the current minimum has to be updated. Push both the new value and the current minimum to the same (or parallel) stack(s). When popping a value from the stack, we can also pop a record of the current minimum at that level, and the new top of the stack(s) will have a record of the previous minimum based on all the values below it.

```markdown
MinStack(): set up a stack that can store two numbers together or two stacks that can each store a number in parallel
push(value): Append value and current minimum to stack(s)
pop(): remove value and current minimum from the top of stack(s)
top(): return value from the top of stack
getMin(): return current minimum from the top of stack
```

⚠️ **Common Mistakes**

* It's easy to get stuck on a strategy for updating the minimum value that will either not work in all cases or is not a constant time operation. Use the stack frame memory to your advantage to solve this problem.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class MinStack:
    def __init__(self):
        self.stack = [ (None, float('inf')) ]
        
    def push(self, val: int) -> None:           
        self.stack.append( (val, min(val, self.getMin())) )

    def pop(self) -> None:
        self.stack.pop()
        
    def top(self) -> int:
        return self.stack[-1][0]

    def getMin(self) -> int:
        return self.stack[-1][1]
```
```java
class MinStack {
    Stack<Integer> mins = new Stack<Integer>();
    Stack<Integer> stack = new Stack<Integer>();
    
    public void push(int val) {
        stack.push(val);
        if (mins.empty() || val < mins.peek()) {
          mins.push(val);
        } else{
          mins.push(mins.peek());
        }
    }
    
    public void pop() {
        stack.pop();
        mins.pop();
    }
    
    public int top() {
        return stack.peek();
    }
    
    public int getMin() {
        return mins.peek();
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(1) for all stack operations
* **Space Complexity**: O(N) total stack space used