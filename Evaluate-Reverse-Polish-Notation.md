## Problem Highlights

* 🔗 **Leetcode Link:** [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Array, Stack
* 🗒️ **Similar Questions**: [Basic Calculator](https://leetcode.com/problems/basic-calculator/), [Expression Add Operators](https://leetcode.com/problems/expression-add-operators/)
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could someone call pop() on an empty Stack?
  - If pop() is called on an empty Stack, it will cause Error.
- What is the input type?
  - The input is a List/Array of String
- What if the list if empty?
    - The size range of the List/Array is from 1 to 10^4. Therefore the input can never be empty.

   
```markdown
HAPPY CASE
Input: ["2","1","+","3","*"]
Output: 9
The expression is evaluated as follows: ((2 + 1) * 3) = 9

Input: ["4","13","5","/","+"]
Output: 6
The expression is evaluated as follows: (4 + (13 / 5)) = 6

EDGE CASE
Input: ["2"]
Output: 2
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
  - No we don't want to interfere with the order of the items in the stack
- Two pointer solutions (left and right pointer variables).
  - No we don't want to swap between a left and right pointer
- Storing the elements of the array in a HashMap or a Set. 
  - No storing element will not help here.
- Traversing the array with a sliding window. 
    - No we are not working with a window constraint.
- Utilize a Stack (Common Data Structure for Parenthesis Problems)
   - This problem follows the standard pattern for postfix notation. Moreover, to get the output, the expression is evaluated to have basic math with Parentheses like this example: ((2 + 1) * 3) = 9

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use Stack to store numbers and help with the evaluation.

```markdown
1. Create a Stack which will store the numbers.
2. When you come across an operator, pop the top two elements from the stack.
3. Create a switch case which will act based on the type of the operator.
4. For each switch case, perform their operations on the two variables and push the result into the stack again.
5. At the end of the traversal, the element at the top of the stack is the result.
```

**⚠️ Common Mistakes**

- The common solution for Array problems are two pointer, hash, sliding window, and sort. Stack is a more unique case, and not utilizing a stack here, will make life harder.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def evalRPN(self, tokens):
    # 1) Create a Stack which will store the numbers.
    stack = []

    for token in tokens:
        if token not in "+-/*":
            stack.append(int(token))
            continue

        # 2) When you come across an operator, pop the top two elements from the stack.
        number_2 = stack.pop()
        number_1 = stack.pop()

        # 3) Create a switch case which will act based on the type of the operator.
        # 4) For each switch case, perform their operations on the two variables and push the result into the stack again.
        result = 0
        if token == "+":
            result = number_1 + number_2
        elif token == "-":
            result = number_1 - number_2
        elif token == "*":
            result = number_1 * number_2
        else:
            result = int(number_1 / number_2)

        stack.append(result)

    # 5) At the end of the traversal, the element at the top of the stack is the result.
    return stack.pop()
        
```
```java
class Solution {
    public int evalRPN(String[] tokens) {
        // 1) Create a Stack which will store the numbers.
        Stack<Integer> stack = new Stack<>();

        for (String token : tokens) {
            if (!"+-*/".contains(token)) {
                stack.push(Integer.valueOf(token));
                continue;
            }
            // 2) When you come across an operator, pop the top two elements from the stack.
            int number2 = stack.pop();
            int number1 = stack.pop();

            // 3) Create a switch case which will act based on the type of the operator.
            // 4) For each switch case, perform their operations on the two variables and push the result into the stack again.
            int result = 0;
            switch (token) {
                case "+":
                    result = number1 + number2;
                    break;
                case "-":
                    result = number1 - number2;
                    break;
                case "*":
                    result = number1 * number2;
                    break;
                case "/":
                    result = number1 / number2;
                    break;
            }
            stack.push(result);
        }
        // 5) At the end of the traversal, the element at the top of the stack is the result.
        return stack.pop();
    }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array

* **Time Complexity**: `O(N)` because we need to traverse all items in the array
* **Space Complexity**: `O(N)` because we may need to store all items in the array into a stack