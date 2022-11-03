## Problem Highlights

* 🔗 **Leetcode Link:** [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/) 
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Stacks, Queues
* 🗒️ **Similar Questions**: [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Should we account for calling pop() and peek(), on an empty Queue?
   - We can assume these functions will always be called on non-empty queues.
- Are we able to use any other data structures other than stacks?
   - We can solve this problem using only two stacks so we should constrain our solution to only use those data structures.
- What are the Time/Space constraints considerations?
   - We should have the same O(1) time complexity for all the standard queue data structure operations and use no more than O(N) total space in our queue


Run through this example case:

```markdown
Input:
["MyQueue", "push", "push", "peek", "pop", "empty", "pop", "empty"]
[       [],    [7],   [-2],     [],    [],      [],    [],      []]
Output:
[     null,   null,   null,     7,     7,   false,     -2,    true]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Stack: What could we push onto a stack to make this problem easier? Stacks don’t allow us to keep track of data based on keys.
- Two Stack Operations: What is unique about having two stacks instead of just one?
- Queue: Queues fall into the same category as Stacks, do we need to maintain any sense of ordering to solve this problem? Queues process values in a first-in-first-out order --- how can we use our stacks to mimic this ordering?
- HashMap: HashMaps allow us to store data for quick access. What could we store in a HashMap to make this problem easier?
- Heap: Do we need some sort of ordering to our data that a Heap could provide?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** For pushing a new value to the queue, push it in the incoming stack. When popping from the queue, check if the outgoing stack is empty. If it is empty then pop all elements from the incoming stack and push them into the outgoing stack once to reverse the ordering. When the outgoing stack is not empty then we can just pop the top element in constant time, making this an amortized O(1) operation. Peeking is the same as popping except we will only return and not pop the top element at the end.


```markdown
push(value): Push value to the intake stack
int pop(): 1. If the output stack has any elements, pop and return the top value.
           2. If the output stack is empty and the intake stack has elements, then pop each element from intake stack and push it onto output stack. Finally pop and return the top value of output stack.
int peek(): Same steps for pop() but only returning the top value
boolean empty(): Returns true if the intake and output stacks are both empty, false otherwise

```

⚠️ **Common Mistakes**

* It's easy to overlook a O(N) time stack transfer operation given the strict O(1) time constraints of the problem. Use timing and space constraints as rough guidelines - if you are ever stuck on a similar problem during an interview don't shy away from communicating the use of any helper functions outside of the given time constraints that may faciliate a solution to your problem. If you're not comfortable with the concept of amortized time complexity, definitely take note of how useful it can be!

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Queue(object):
    def __init__(self):
        self.inStack, self.outStack = [], []

    def push(self, x):
        self.inStack.append(x)

    def pop(self):
        self.move()
        return self.outStack.pop()

    def peek(self):
        self.move()
        return self.outStack[-1]

    def empty(self):
        return (not self.inStack) and (not self.outStack)

    def move(self):
        if not self.outStack:
            while self.inStack:
                self.outStack.append(self.inStack.pop())
```
```java
class MyQueue {

    // initialize your data structure here
    Stack<Integer> input;
    Stack<Integer> output;
    public MyQueue() {
         input = new Stack<>();
         output = new Stack<>();
    }
    
    // push element x to the back of queue
    public void push(int x) {
        input.push(x);
    }
    
    // removes the element from in front of queue and returns that element
    public int pop() { 
        // we need to reverse the input sequence for getting result acc to queue
        if(output.size() > 0){
            return output.pop();
        }else{
            while(input.size() > 0){
                output.push(input.pop());
            }
            
            return output.pop();
        }
    }
    
    // get the front element
    public int peek() {
        // we need to reverse the input sequence to get result acc to queue
        if(output.size() > 0){
            return output.peek();
        }else{
            while(input.size() > 0){
                output.push(input.pop());
            }
            
            return output.peek();
        }
    }
    
    // returns whether the queue is empty
    public boolean empty() {
        return (input.size() == 0 && output.size() == 0) ;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(1) for all queue operations, (amortized O(1) for pop() and peek())
* **Space Complexity**: O(N) total queue space used