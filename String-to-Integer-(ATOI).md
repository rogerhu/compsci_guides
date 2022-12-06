## Problem Highlights

* 🔗 **Leetcode Link:** [String to Integer (atoi)
](https://leetcode.com/problems/string-to-integer-atoi/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Strings, Math
* 🗒️ **Similar Questions**: [Check if Numbers Are Ascending in a Sentence](https://leetcode.com/problems/check-if-numbers-are-ascending-in-a-sentence/), [Reverse Integer](https://leetcode.com/problems/reverse-integer/), [Valid Number](https://leetcode.com/problems/valid-number/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could the input parameter be Null?

  - No. In addition, let’s assume the input is a valid number and contains no alphabetical characters. The only input characters will be the negative sign “-” and integers.


- Could the input string represent negative numbers?

  - Yes

- Could the input string represent numbers larger/smaller than a computer can represent with a standard integer data type?
    - For this problem, let’s assume the input numbers will be able to be represented in a computer.

- Could the input string represent have characters other than numbers?
    - We stop building the integer once we see a non-numeric character .
   
```markdown
HAPPY CASE
Input: "123"
Output: 123

Input: "-6714"
Output: -6714

EDGE CASE
Input: "what is a 1000"
Output: 0
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Sort
    - Does sorting help us achieve what we need in order to solve the problem?

- Two pointer solutions (left and right pointer variables)
    - Can two or multiple pointers help us in any way?
- Storing the elements of the array in a HashMap or a Set
    - In reversing, hashing elements and storing them may not yield an optimal solution.

- Traversing the array with a sliding window
    - Will viewing pieces of the input at a time help us?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Iterate from left to right and multiply the integer by 10 each time to shift the number.


```markdown
1) Clean up string
2) Create boolean variable for negatives
3) Create an integer variable
4) Iterate the input string from left to right as long as digit is seen
    a) Multiply the integer variable by 10 and add the current indexed integer in the string
5) Handles negative case
6) Handle index out of bound case
7) Return Built Int
```

⚠️ **Common Mistakes**

* This problem deals with more of math intuition rather than string operations. During this step, encourage students to dive deeper than just strings to see other ways they can solve this problem.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def myAtoi(self, s: str) -> int:

        # Clean up string
        s = s.strip()
        if not s:
            return 0
        
        # Create boolean variable for negatives
        isNeg = False
        if s[0] == "-":
            isNeg = True
            s = s[1:]
        elif s[0] == "+":
            s = s[1:]
        
        # Create an integer variable
        buildInt = 0
        i = 0

        # Iterate the input string from left to right as long as digit is seen
        while i < len(s) and s[i].isdigit():
            # Multiply the integer variable by 10 and add the current indexed integer in the string
            buildInt = buildInt * 10 + int(s[i])
            i += 1
        
        # Handles negative case
        buildInt = -1 * buildInt if isNeg else buildInt

        # Handle index out of bound case
        buildInt =  -2**31 if buildInt < -2**31 else buildInt
        buildInt =   2 ** 31 - 1 if buildInt > 2 ** 31 - 1 else buildInt

        # Return Built Int
        return buildInt
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume N represents the number of character in the string.


* **Time Complexity**: O(n), traversing may be done on every character in string. 
* **Space Complexity**: O(n), we will be building a integer with the length of the string.