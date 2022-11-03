## Problem Highlights

* 🔗 **Leetcode Link:** [Validate Stack Sequences](https://leetcode.com/problems/validate-stack-sequences/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Array, Stack, Greedy
* 🗒️ **Similar Questions**: TBD
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What are we validating?
  - We return true if the sequence is the result of push and pop operations on an initially empty stack, or false otherwise.
- We have to push the items in order, so when do we pop them?
  - If the stack has say, 2 at the top, then if we have to pop that value next, we must do it now. That's because any subsequent push will make the top of the stack different from 2, and we will never be able to pop again.
- What if values in pushed and popped are not distinct?
  - At every step, you can either push a value (unconditionally), or pop a value (if it matches the popped order).

Run through a set of example cases:

```markdown
HAPPY CASE
Input: pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
Output: true

Input: pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
Output: false

EDGE CASE
Input: pushed = [0], popped = [0]
Output: true
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Stack: What could we push onto a stack to make this problem easier? Stacks don’t allow us to keep track of data based on keys.
- Queue: Queues fall into the same category as Stacks, do we need to maintain any sense of ordering to solve this problem?
- HashMap: HashMaps allow us to store data for quick access. What could we store in a HashMap to make this problem easier?
- Heap: Do we need some sort of ordering to our data that a Heap could provide?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** For each value, push it to the stack. Then, pop values from the stack if they are the next values to pop. At the end, we check if we have popped all the values successfully.

```markdown
1. use an actual stack to verify the input
2. have a pointer index for each array to keep track of our operations
3. if the current val in pushed does not match that of popped, push that val
4. else if they do match, then pop
5. if after popping, the popped val does not match, then this is an incorrect popped arr and return false
6. after going through both the pushed arr with no issues, we should check our stack
7. start popping each val from the stack and compare it to the remaining values of the popped array. 
8. if there are no mismatched results, success!
```

⚠️ **Common Mistakes**

* We can miss some invalid sequences including but not limited to: 
1. the lengths of the arrays are unequal (number of pushes != number of pops): This case is handled by the question itself by giving pushed.size() == popped.size().
2. The pushed numbers are different than the popped numbers: This case is also handled by the question itself as we are given that the pushed and popped arrays are permutations of each other.
3. There is at least one element in popped that can't come at the top of the stack (while ensuring all elements before it in the popped array have been popped in sequence): This means that if we were to simulate a stack, we couldn't pop this element while maintaining the previous order.
4. There is at least one element in the pushed array that doesn't get popped after it has been pushed: This means that either we have done a mistake before, or this element is simply out of order.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python

```
```java
public boolean validateStackSequences(int[] pushed, int[] popped) {
        int n = pushed.length;
        int i = 0;
        Stack myStack = new Stack<>();

        // push each int num in pushed arr
        for (int x : pushed) {
            myStack.push(x);
            // if stack has elements
            // and we have not iterated thru all popped nums
            // and the top of the stack matches the current popped num
            while (!myStack.isEmpty() && i < n && myStack.peek() == popped[i]) {
                // pop, increment index of popped
                myStack.pop();
                i++;
            }
        }
        // if i equals n, then we popped successfully all nums in popped arr
        // else we could not pop everything
        return i == n;
    }    
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: 
* **Space Complexity**: 