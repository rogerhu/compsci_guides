## Problem Highlights

* 🔗 **Geeksforgeeks Link:**  [Sort a stack using a temporary stack](https://www.geeksforgeeks.org/sort-stack-using-temporary-stack/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Array, Stack
* 🗒️ **Similar Questions**: [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/), [Check if a Parentheses String Can Be Valid](https://leetcode.com/problems/check-if-a-parentheses-string-can-be-valid/)
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What if the Stack is empty?
    - Then the Stack already sorted, return the Stack

- What type of the number inside the Stack?
    - It only contains integer
   
```markdown
HAPPY CASE
Input: [10,9,3,1]
Output: [1,3,9,10]

Input: [1,2,3]
Output: [1,2,3]

EDGE CASE
Input: []
Output: []
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For brackets problems, we want to consider the following approaches:

- Using a counter: 
    - We are not counting anything
- Utilize a Stack: 
    - Stack is useful here since we are required to use a temporary stack 

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use additional Stack to store number in the order when it get pop out of the input Stack.



```markdown
1. Create an additional temporary Stack.
2. While input stack is NOT empty do:
3. Pop an element from input stack called temp.
4. While temporary stack is NOT empty and top of temporary stack is greater than temp:
5. Pop from temporary stack and push it to the input stack.
6. Push temp in temporary stack.
7. In the end, the sorted numbers are in the temporary Stack.
```

**⚠️ Common Mistakes**

* Look out to pop and push element to the right Stack
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def sortStack(self, input):
        # 1. Create an additional temporary Stack.
        tempStack = []

        # 2. While input stack is NOT empty do:
        while len(input) != 0:
            # 3. Pop an element from input stack called temp.
            temp = input.pop()

            # 4. While temporary stack is NOT empty and top of temporary stack is greater than temp:
            while len(tempStack) != 0 and tempStack[len(tempStack)-1] > temp:
                # 5. Pop from temporary stack and push it to the input stack.
                input.append(tempStack[len(tempStack)-1])
                tempStack.pop()

            # 6. Push temp in temporary stack.
            tempStack.append(temp)

        # 7. In the end, the sorted numbers are in the temporary Stack.
        return tempStack
```
```java
class Solution {
    public static Stack<Integer> sortStack(Stack<Integer> input) {
        // 1. Create an additional temporary Stack.
        Stack<Integer> tempStack = new Stack<Integer>();

        // 2. While input stack is NOT empty do:
        while(!input.isEmpty()) {
            // 3. Pop an element from input stack called temp.
            int temp = input.pop();

            // 4. While temporary stack is NOT empty and top of temporary stack is greater than temp:
            while(!tempStack.isEmpty() && tempStack.peek() > temp) {
                // 5. Pop from temporary stack and push it to the input stack.
                input.push(tempStack.pop());
            }

            // 6. Push temp in temporary stack.
            tempStack.push(temp);
        }

        // 7. In the end, the sorted numbers are in the temporary Stack.
        return tempStack;
    }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of elements in the stack

* **Time Complexity**: `O(N^2)` because we may need to pop each element in the stack n times.
* **Space Complexity**: `O(N)` because we may need to store all items in the array into a temp stack