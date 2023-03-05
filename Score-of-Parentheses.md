## Problem Highlights

* 🔗 **Leetcode Link:** [Score of Parentheses](https://leetcode.com/problems/score-of-parentheses/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 30 mins
* 🛠️ **Topics**: Array, Stack
* 🗒️ **Similar Questions**: [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/), [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input be an empty string?
    - Yes, the input can be an empty string. In the empty case, let’s return 0 as the score.
- Can the input set of parentheses be invalid or unbalanced?
    - No, the input will be a valid string of well-formed parentheses.
```markdown
HAPPY CASE
Input: "(())"
Output: 2

Input: "(()(()))"
Output: 6

EDGE CASE (Empty String)
Input: ""
Output: 0 
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For brackets problems, we want to consider the following approaches:

- Using a counter: 
    - A global score counter could help us keep track of the score. However, the number of left/right tokens would not help us to determine the score since there is more to score ruling than simply the number of left/right parentheses.
- Utilize a Stack: 
    - A Stack could help keep track of where certain pairs of parentheses evaluate to sub-scores and where incomplete pairs are located. What could we push onto a stack to make this problem easier? Stacks don’t allow us to keep track of data based on keys.
- HashMap: HashMaps allow us to store data for quick access. What could we store in a HashMap to make this problem easier?
- Heap: Do we need some sort of ordering to our data that a Heap could provide?


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** (STACK) Iterate through input string and use a Stack to keep track of all of the computations and incomplete pairings.

```markdown
1) Create a Stack
2) Iterate through the input string from left to right:
    a) If we see a "(":
        i) If we see a "(": push a 0 into stack
    b) If we see a ")":
        i) Add current value to previous value
3) Return the only item which equals total value of stack.
```

**General Idea:** (Core Count) Iterate through input string and keep track of the number of open parenthesis this represents the number of exterior set of parentheses that contains this core. If we close this core lets use bit manipulation to double the value according to the number of exterior set of parentheses and add it to the total.
```markdown 
1. Initialize the total and current balance
2. Iterate through the string and keep track of the number of exterior set of parentheses using balance
    a. Upon seeing a opening parentheses: add to balance
    b. Upon seeing a closing parentheses: reduce the balance
        i. If it happens to close, because the previous item was an opening parentheses. 
        ii. Then this core needs to be added to the total and it's value is doubled with each exterior set of parentheses(aka the balance). 
3. Return the total
```

**⚠️ Common Mistakes**

* Are the parenthesis the only things we can add to a stack?


## 4: I-mplement

> **Implement** the code to solve the algorithm.

**Approach #1 Stacks**
```python
class Solution:
    def scoreOfParentheses(self, S: str) -> int:
        # Create a Stack
        stack = [0]
        # Iterate through the input string from left to right
        for c in s:
            # If we see a "(": push a 0 into stack
            if c == "(":
                stack.append(0)
            # Else we see a ")": add current value to previous value
            else:
                enclosedVal = stack.pop()
                stack[-1] += max(2 * enclosedVal, 1)
        # Return the only item which equals total value of stack
        return stack.pop()
```
**Approach #2 Core Count**
```python
class Solution:
    def scoreOfParentheses(self, s: str) -> int:
        # Initialize the total and current balance
        total = bal = 0

        # Iterate through the string and keep track of the number of exterior set of parentheses using balance
        for i, x in enumerate(s):
            # Upon seeing a opening parentheses: add to balance
            if x == '(':
                bal += 1
            # Upon seeing a closing parentheses: reduce the balance
            else:
                bal -= 1
                # If it happens to close, because the previous item was an opening parentheses.
                if s[i-1] == '(':
                    # Then this core needs to be added to the total and it's value is doubled with each exterior set of parentheses(aka the balance)
                    total += 1 << bal
                    
        # Return the total
        return total
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the string

* **Time Complexity**: `O(N)` because we need to traverse all items in the string
* **Space Complexity**: `O(N)` or `O(1)` 
    * **Stack** We may need to store all items in the array into a stack for `O(N)` space.
    * **Core Count** method to complete the problem in `O(1)` Space. 