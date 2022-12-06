## Problem Highlights

* 🔗 **Leetcode Link:** [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Array, Stack, Queue
* 🗒️ **Similar Questions**: , [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/), [Build an Array With Stack Operations](https://leetcode.com/problems/build-an-array-with-stack-operations/)
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could someone call pop() on an empty Queue?
    - “All operations called on your Queue will be valid, so a call to pop() won’t occur if the Queue is empty.”
- How do we use a stack in Python?
    - “You could use a standard list in Python since it supports stack like functions like append() and pop().”
    - 
```markdown
HAPPY CASE
q = Queue()
q.push(5) -> No Return
q.push(2) -> No Return
q.peek() -> 5
q.pop() -> 5
q.empty() -> False
q.pop() -> 2

q = Queue()
q.push(1) -> No Return
q.pop() -> 1
q.empty() -> True
q.empty() -> True

EDGE CASE
q = Queue()
q.empty() -> True
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

This problem does not follow a strict, standard pattern. However, urge students to consider the fundamental differences between Stacks and Queues:

- Order of Data
  - This is a critical difference that can be used to solve this problem
- Runtimes of Access/Placement operations

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use 2 Stacks to pass data between to emulate a Queue

```markdown
Constructor
1) Create 2 Stacks, a main and a side

Push
1) Push elements onto the main Stack, never the side

Pop/Poll
0) Return element in side stack if not empty
1) Pop all elements off of the main Stack onto the side Stack, this reverses
    element order to that of a Queue
2) Return element in side stack if not empty

Peek
0) Return the first element in the side Stack if not empty
1) Pop all elements off of the main Stack onto the side Stack, this reverse element order to that of a Queue
2) Return the first element in the side Stack if not empty

Empty
1) Return True if main Stack empty and side Stack is empty, else False
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python

class MyStack:
    
    def __init__(self):
        self._stack = []
    
    def push(self, x: int) -> None:
        self._stack.append(x)

    def pop(self) -> int:
        return self._stack.pop()
    
    def peek(self) -> int:
        return self._stack[-1]
    
    def size(self) -> int:
        return len(self._stack)
    
    def empty(self) -> bool:
        return len(self._stack) == 0
    
class MyQueue:

    def __init__(self):
        # Create 2 Stacks, a main and a side
        self.popstack = MyStack()
        self.pushstack = MyStack()

    def push(self, x: int) -> None:
        # Push elements onto the main Stack, never the side
        self.pushstack.push(x)

    def pop(self) -> int:
        
        # Return element in side stack if not empty
        if self.popstack.size() > 0:
            return self.popstack.pop()
        
        # Pop all elements off of the main Stack onto the side Stack, this reverses
        while not self.pushstack.empty():
            self.popstack.push(self.pushstack.pop())
        
        # Return element in side stack if not empty
        if self.popstack.empty():
            return None
        return self.popstack.pop()
        

    def peek(self) -> int:
        # Return the first element in the side Stack
        if self.popstack.size() > 0:
            return self.popstack.peek()
        
        # Pop all elements off of the main Stack onto the side Stack, this reverses element order to that of a Queue
        while not self.pushstack.empty():
            self.popstack.push(self.pushstack.pop())
        
        # Return the first element in the side Stack if not empty
        if self.popstack.empty():
            return None
        return self.popstack.peek()

    def empty(self) -> bool:
        # Return True if main Stack empty and side Stack is empty, else False
        return self.popstack.empty() and self.pushstack.empty()
        

```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the string

* **Time Complexity**: `O(N)` because we will need to pop all elements to emulate queue's dequeue for pop and peek operations. All other operations are O(1)

* **Space Complexity**: `O(N)` because we will need to store a queue items into the main and side stacks.