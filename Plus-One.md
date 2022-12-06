## Problem Highlights

* 🔗 **Leetcode Link:** [Plus One](https://leetcode.com/problems/plus-one/) 
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array
* 🗒️ **Similar Questions**: [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/), [Maximum Product of the Length of Two Palindromic Subsequences](https://leetcode.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences/), [Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input represent a negative number?

  - No. Each digit ranges from 0 to 9 inclusive.

- What exactly is an arbitrary precision integer?
  - Precision refers to the number of digits in a number. An arbitrary precision integer is an integer "whose digits of precision are limited only by the available memory of the host system" (i.e. the precision, or number of digits, is only limited by the available memory)

- Any space and time constraints?
    - Try to solve this with O(1) space.

   
```markdown
HAPPY CASE
Input: [1,2,3]
Output: [1,2,4]
Explanation: 123 + 1 = 124

Input: [5,8,9]
Output: [5,9,0]
Explanation: 589 + 1 = 590

EDGE CASE
Input: digits = [9]
Output: [1,0]
Explanation: The array represents the integer 9.
Incrementing by one gives 9 + 1 = 10.
Thus, the result should be [1,0].
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Sort
    - Does sorting help us achieve what we need in order to solve the problem?
- Two pointer solutions (left and right pointer variables)
    - Can two or multiple pointers help us in any way?
- Storing the elements of the array in a HashMap or a Set
    - Do we need additional data structures to solve this problem? If yes, which ones?
- Traversing the array with a sliding window
    - Will viewing pieces of the input at a time help us?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Add one to the last digit and while remainder keep adding 1 until the first digit. If there still is a remainder, then insert a 1 at the beginning of the array

```markdown
1. Add one to the last digit 
2. While there is a remainder, add to remaining digits until first digit
3. If there is still a remainder, insert a 1 to the beginning of the array
4. Return digits
```

⚠️ **Common Mistakes**

* Watch out for the parentheses 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        # Add one to the last digit 
        i = len(digits) - 1
        remainder = (digits[i] + 1) // 10 
        digits[i] = (digits[i] + 1) % 10
        i -= 1

        # While there is a remainder, add to remaining digits until first digit
        while i >= 0 and remainder:

            remainder = (digits[i] + 1) // 10 
            digits[i] = (digits[i] + 1) % 10
            i -= 1

        # If there is still a remainder, insert a 1 to the beginning of the array
        if i < 0 and remainder:
            digits.insert(0, 1)

        # Return digits
        return digits

```
```java
class Solution {
    public int[] plusOne(int[] digits) {
        //  Add one to the last digit 
        int i = digits.length - 1;
        int remainder = (digits[i] + 1) / 10 ;
        digits[i] = (digits[i] + 1) % 10;
        i--;

        // While there is a remainder, add to remaining digits until first digit
        while(i >= 0 && remainder > 0) {
            remainder = (digits[i] + 1) / 10 ;
            digits[i] = (digits[i] + 1) % 10;
            i--;
        }

        // If there is still a remainder, insert a 1 to the beginning of the array
        if (i < 0 && remainder > 0) {
            digits=new int[digits.length+1];
            digits[0]=1;
        }

        // Return digits
        return digits;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume N represents the number of elements in the array.


* **Time Complexity**: O(n), traversing may be done on every number. 
* **Space Complexity**: O(1), we do not need any extra memory here, because we reuse the digits array.