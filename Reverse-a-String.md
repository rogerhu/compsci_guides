## Problem Highlights

* 🔗 **Leetcode Link:** [Reverse a String](https://leetcode.com/problems/reverse-string/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Strings, Two Pointer
* 🗒️ **Similar Questions**: [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/), [Reverse String II](https://leetcode.com/problems/reverse-string-ii/), [Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:
* Is the input only a string?
* Can elements of the string be both characters and numbers?
* Have you verified any Time/Space Constraints for this problem?

Run through a set of example cases:

```markdown
HAPPY CASE
Input: "REVERSE" 
Output: "ESREVER" 

Input: "42 Wallaby Way, Sydney" 
Output: "yendyS ,yaW yballaW 24"

EDGE CASE
Input: ""
Output: ""
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Strings, common solution patterns include:
* Two pointer solutions (left and right pointer variables): What we can do is take two pointer variables: start and end pointers. Then, point them with the two ends of the input string. When left pointer and right pointer pointing at the same index, we should exit and return the reverse string
* Sort the character in the string.
* Storing the characters of the string in a HashMap or a Set
* Traversing the string with a sliding window

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Start traversing inwards, from both ends of the input string, and we can expect to swap the characters, in the same order.

```markdown
1. We first convert the string into an array, because a string is immutable.
2. Then we can take two pointer variables, start and end and point them with the two ends of the array.
3. As we move the start pointer right and end pointer left, we swap the characters.
4. After loop finishes, the string is said to be reversed, hence convert the array into a string and return.
```
⚠️ **Common Mistakes**
* A less optimized approached would be build a new string iterating through the string in reverse. We will need perform O(n) loops rather than O(n/2) loops.


## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
#Approach 1: Two Pointer

class Solution:
    def reverseString(self, s: List[str]) -> None:
        # We first convert the string into an array, because a string is immutable.
        arr = list(input_str)

        # Then we can take two pointer variables, start and end and point them with the two ends of the array.
        l, r = 0, len(arr) - 1

        # As we move the start pointer right and end pointer left, we swap the characters.
        while l < r:
            arr[l], arr[r] = arr[r], arr[l]
            l, r = l + 1, r - 1

        # After loop finishes, the string is said to be reversed, hence convert the array into a string and return.
        return "".join(arr)
```

```python
#Approach 2: Iterative

class Solution:
    def reverseString(self, s: List[str]) -> None:
        output_str = ""
        for i in range(len(input_str) - 1, -1, -1):
            output_str += input_str[i]
        return output_str
```

⚠️ **Common Mistakes**
* String concatenation is dependent on language usage.
* Java StringBuilder allows for O(1) appends
* Python does not have a common StringBuilder class to use    

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(n), traversing is done on every character, until the pointers meet in the middle. Time complexity will be O(n) (where n is the length of the given string).

* **Space Complexity**: O(n), we need memory to hold new string or new array, because python strings are immutable. 
