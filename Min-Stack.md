## Problem Highlights

* 🔗 **Leetcode Link:** [Min Stack](https://leetcode.com/problems/min-stack/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Stack
* 🗒️ **Similar Questions**: [Max Stack](https://leetcode.com/problems/max-stack/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Should we account for calling pop(), top(), or getMin() on an empty MinStack?
   - We can assume these functions will always be called on non-empty stacks.
- Should pop() also return the top element of the MinStack?
   - Nope, but good attention to detail! In this case, pop only removes the top element and does not return anything.
- Besides implementing a common stack structure it is important to consider how we might update the new minimum after popping off the old so that we can easily return the newly updated minimum value in constant time.
- What are the Time/Space constraints considerations?
   - We are constrained to O(1) time for all MinStack operations, but how could different uses of O(N) space solve this problem?

```markdown
HAPPY CASE
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

HARDER CASE

Input: 
["MinStack","push","push","push","push","getMin","pop","top","getMin"]
[        [],   [1],  [-1],  [2],   [-8],      [],   [],   [],      []]
Output:
[      null,  null,  null, null,  null,       -8, null,    2,      -1]
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