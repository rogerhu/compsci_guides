## Problem Highlights

* 🔗 **Leetcode Link:** N/A
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Strings, Math
* 🗒️ **Similar Questions**: [String to Integer (atoi)
](https://leetcode.com/problems/string-to-integer-atoi/), [Check if Numbers Are Ascending in a Sentence](https://leetcode.com/problems/check-if-numbers-are-ascending-in-a-sentence/), [Reverse Integer](https://leetcode.com/problems/reverse-integer/), [Valid Number](https://leetcode.com/problems/valid-number/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could the input parameter be Null?

  - Let’s assume the input is a valid integer.

- Could the input string represent negative numbers?

  - Yes, the input can be negative.

```markdown
HAPPY CASE
Input: -5
Output: "-5"

Input: 4
Output: "4"

EDGE CASE
Input: 0
Output: "0"
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
1) Create an array
2) Set a negative flag if input value is negative
3) Iterate through the integer backwards through divide by 10 and modulus method
    a) Append element to array
4) Reverse the array and join the elements into a string
5) Return newly created string
```

⚠️ **Common Mistakes**

* This problem deals with more of math intuition rather than string operations. During this step, encourage students to dive deeper than just strings to see other ways they can solve this problem.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def int_to_string(num: int) -> str:
    if num == 0:
        return "0"
    
    # Set a negative flag if input value is negative
    negative_flag = num < 0
    num = abs(num)
    
    # Create an array
    arr = []
    
    # Iterate through the integer backwards through divide by 10 and modulus method
    while num != 0:
        # Append element to array
        arr.append(chr(num % 10 + ord('0')))
        num = num // 10
        
    # Reverse the array and join the elements into a string
    arr.reverse()
    return "-" + "".join(arr) if negative_flag else "".join(arr) 
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume N represents the number of character in the string.


* **Time Complexity**: O(n), traversing done on every digit in integer 
* **Space Complexity**: O(n), we will be building a string with the length of the number.