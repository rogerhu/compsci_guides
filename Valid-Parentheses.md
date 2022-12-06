## Problem Highlights

* 🔗 **Leetcode Link:** [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array, Stack
* 🗒️ **Similar Questions**: [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/), [Check if a Parentheses String Can Be Valid](https://leetcode.com/problems/check-if-a-parentheses-string-can-be-valid/)

## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What if the String if empty?
    - The size range of the List/Array is from 1 to 10^4. Therefore the input can never be empty.

   
```markdown
HAPPY CASE
Input: "()[]{}"
Output: true

Input: "(]"
Output: false

EDGE CASE
Input: "("
Output: false
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For brackets problems, we want to consider the following approaches:

- Using a counter: 
    - We can not use this method since we have 3 types of brackets so with a counter, we can not use one counter to keep track all 3 types.
- Utilize a Stack: 
    - Stack is useful here since we can easily keep track of the last bracket.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use Stack to store brackets and check if open and close bracket matching with each other or not.

```markdown
1. Create a Stack which will store the brackets.
2. Process each bracket of the expression one at a time.
3. If encounter a closing bracket, then check the element on top of the stack. If the element at the top of the stack is an opening bracket of the same type, then pop it off the stack and continue processing. Else, this implies an invalid expression.
4. If encounter an opening bracket, push it onto the stack.
5. In the end, if we are left with a stack still having elements, then this implies an invalid expression.
```

**⚠️ Common Mistakes**

* Look out for edge case
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def isValid(self, s):
        if s == "":
            return True

        # Create a Stack which will store the brackets.
        stack = [s[0]]

        # Process each bracket of the expression one at a time.
        for i in range(1,len(s)):
            # If encounter a closing bracket, then check the element on top of the stack. 
            if s[i] == ')':
                # If the element at the top of the stack is an opening bracket of the same type, then pop it off the stack and continue processing. Else, this implies an invalid expression. 
                if stack and stack[-1] == '(':
                    stack.pop()
                else:
                    return False
            elif s[i] == ']':
                if stack and stack[-1] == '[':
                    stack.pop()
                else:
                    return False
            elif s[i] == '}':
                if stack and stack[-1] == '{':
                    stack.pop()
                else:
                    return False
            else:
                # If encounter an opening bracket, push it onto the stack.
                stack.append(s[i])

        # In the end, if we are left with a stack still having elements, then this implies an invalid expression.
        return not stack```
```java
class Solution {
   public boolean isValid(String s) {
        // Hash table that takes care of the mappings between open brackets and close brackets
        HashMap<Character, Character> mappings = new HashMap<Character, Character>();
        mappings.put(')', '(');
        mappings.put('}', '{');
        mappings.put(']', '[');

        // 1. Create a Stack which will store the brackets.
        Stack<Character> stack = new Stack<Character>();

        // 2. Process each bracket of the expression one at a time.
        for (int i = 0; i < s.length(); i++) {
          char c = s.charAt(i);

          // 3. If encounter a closing bracket, then check the element on top of the stack.
          // If the element at the top of the stack is an opening bracket of the same type,
          // then pop it off the stack and continue processing. Else, this implies an invalid expression.
          if (mappings.containsKey(c)) {

            // Get the top element of the stack. If the stack is empty, set a dummy value of '#'
            char topElement = stack.empty() ? '#' : stack.pop();

            if (topElement != mappings.get(c)) {
              return false;
            }
          } else {
            // 4. If encounter an opening bracket, push it onto the stack.
            stack.push(c);
          }

        // 5. In the end, if we are left with a stack still having elements, then this implies an invalid expression.
        return stack.isEmpty();
    }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the string

* **Time Complexity**: `O(N)` because we may need to traverse all items in the string
* **Space Complexity**: `O(N)` because we may need to store all items in the array into a stack