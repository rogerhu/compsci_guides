## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/dinner-plate-stacks/>
* 💡 **Problem Difficulty:** Hard
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Stacks, Heaps
* 🗒️ **Similar Questions**: TBD
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- What data structure can we use to keep track of the first stack? 
  - Use a heap to keep track of the first stack that is available. The cost of pop of an element from the heap can be charged to the pop()/popAtStack() operation that pushed the element into the heap. 
- What are the time and space constraints?
  - 1 <= capacity <= 2 * 104
  - 1 <= val <= 2 * 104
  - 0 <= index <= 105
  - At most 2 * 105 calls will be made to push, pop, and popAtStack.

Run through a set of example cases:

```markdown
HAPPY CASE
Input
["DinnerPlates", "push", "push", "push", "push", "push", "popAtStack", "push", "push", "popAtStack", "popAtStack", "pop", "pop", "pop", "pop", "pop"]
[[2], [1], [2], [3], [4], [5], [0], [20], [21], [0], [2], [], [], [], [], []]
Output:
[null, null, null, null, null, null, 2, null, null, 20, 21, 5, 4, 3, 1, -1]

Input: ["DinnerPlates","push","push","push","push","push","push","push","popAtStack","popAtStack","popAtStack","popAtStack","popAtStack","push","push","pop","pop","pop","pop","pop"]
[[2],[1],[2],[3],[4],[5],[6],[7],[2],[2],[1],[1],[0],[8],[9],[],[],[],[],[]]
Output:
[null,null,null,null,null,null,null,null,6,5,4,3,2,null,null,9,7,8,1,-1]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Stacks: follows the LIFO (Last-In-First-Out) principle. Stack has one end, whereas the Queue has two ends (front and rear). It contains only one pointer top pointer pointing to the topmost element of the stack. Whenever an element is added in the stack, it is added on the top of the stack, and the element can be deleted only from the stack. In other words, a stack can be defined as a container in which insertion and deletion can be done from the one end known as the top of the stack.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use a list stacks to save all stacks. Use a heap queue q to find the leftmost available stack.

```markdown
1) To push, we need to find the leftmost available position. First, let's remove any stacks on the left that are full
a) self.q: if there is still available stack to insert plate
b) self.q[0] < len(self.stacks): the leftmost available index self.q[0] is smaller than the current size of the stacks
c) len(self.stacks[self.q[0]]) == self.c: the stack has reached full capacity
2) To pop, we need to find the rightmost nonempty stack
a) stacks is not empty (self.stacks) and 
b) the last stack is empty
3) To pop from an stack of given index, we need to make sure that it is not empty
a) the index for inserting is valid and，
b) the stack of interest is not empty
```

⚠️ **Common Mistakes**

* Having two extra-variable for storing leftmost non-full stack and right most non-empty stack. Leftmost non-full stack variable will show the index of the leftmost stack which is not fully filled means that stack size is not equal to capacity, so try to use this variable to push the value to the leftmost stack efficiently. Rightmost non-empty stack variable will show the index of the rightmost stack which is not empty so using that variable we can efficiently perform the pop from the rightmost stack.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class DinnerPlates:
    def __init__(self, capacity):
        self.c = capacity
        self.q = [] # record the available stack, will use heap to quickly find the smallest available stack
		# if you are Java or C++ users, tree map is another good option.
        self.stacks = [] # record values of all stack of plates, its last nonempty stack are the rightmost position

    def push(self, val):
        # To push, we need to find the leftmost available position
        # first, let's remove any stacks on the left that are full
        # 1) self.q: if there is still available stack to insert plate
        # 2) self.q[0] < len(self.stacks): the leftmost available index self.q[0] is smaller than the current size of the stacks
        # 3) len(self.stacks[self.q[0]]) == self.c: the stack has reached full capacity
        while self.q and self.q[0] < len(self.stacks) and len(self.stacks[self.q[0]]) == self.c:
            # we remove the filled stack from the queue of available stacks
            heapq.heappop(self.q)
            
        # now we reach the leftmost available stack to insert
        
        # if the q is empty, meaning there are no more available stacks
        if not self.q:
            # open up a new stack to insert
            heapq.heappush(self.q, len(self.stacks))
            
        # for the newly added stack, add a new stack to self.stacks accordingly
        if self.q[0] == len(self.stacks):
            self.stacks.append([])
            
        # append the value to the leftmost available stack
        self.stacks[self.q[0]].append(val)

    def pop(self):
        # To pop, we need to find the rightmost nonempty stack
        # 1) stacks is not empty (self.stacks) and 
        # 2) the last stack is empty
        while self.stacks and not self.stacks[-1]:
            # we throw away the last empty stack, because we can't pop from it
            self.stacks.pop()
            
        # now we reach the rightmost nonempty stack
		
        # we pop the plate from the last nonempty stack of self.stacks by using popAtStack function
        return self.popAtStack(len(self.stacks) - 1)

    def popAtStack(self, index):
        # To pop from an stack of given index, we need to make sure that it is not empty
        # 1) the index for inserting is valid and，
        # 2) the stack of interest is not empty
        if 0 <= index < len(self.stacks) and self.stacks[index]:
            # we add the index into the available stack
            heapq.heappush(self.q, index)
            # take the top plate, pop it and return its value
            return self.stacks[index].pop()
        
        # otherwise, return -1 because we can't pop any plate
        return -1       
```
```java
class DinnerPlates {
    int cap;
    vector<stack<int>> stks;
    int leftMost, rightMost;
public:
    DinnerPlates(int capacity) {
        cap = capacity;
        leftMost = 0;
        rightMost = 0;
    }
    
    void push(int val) {
        // traverse the leftMost 
        while(leftMost < stks.size() and stks[leftMost].size() == cap) {
            leftMost++;
        }
        
        // at the end
        if(leftMost == stks.size()) {
            stack<int> stk;
            stk.push(val);
            stks.push_back(stk);
        }
        else { // stack already created
            stks[leftMost].push(val);
        }
        
        // finally update the rightMost value
        rightMost = max(leftMost, rightMost);
    }
    
    int pop() {
        while(rightMost >= 0 and stks[rightMost].empty()) {
            rightMost--;
        }
        
        if(rightMost < 0) return -1;
        
        int top = stks[rightMost].top();
        stks[rightMost].pop();
        leftMost = min(leftMost, rightMost);
        return top;
    }
    
    int popAtStack(int index) {
        //if the particular stack is not present in stks
        if(stks.size() <= index) {
            return -1;
        }
        
        // if the stack is empty at given index
        if(stks[index].empty()) {
            return -1;
        }
        
        // if stack is not empty then return the value
        int tp = stks[index].top();
        stks[index].pop();
        
        leftMost = min(leftMost, index);
        
        return tp;
    }
};

/**
 * Your DinnerPlates object will be instantiated and called as such:
 * DinnerPlates* obj = new DinnerPlates(capacity);
 * obj->push(val);
 * int param_2 = obj->pop();
 * int param_3 = obj->popAtStack(index);
 */     
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: 

   - push, `O(logN)` time
   - pop, amortized `O(1)` time
   - popAtStack, `O(logN)` time

* **Space Complexity**: O(n) since a stack data structure was used
