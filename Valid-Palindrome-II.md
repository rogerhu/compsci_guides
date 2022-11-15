## Problem Highlights

* 🔗 **Leetcode Link:** [Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: 
* 🗒️ **Similar Questions**: [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/), [Valid Palindrome IV](https://leetcode.com/problems/valid-palindrome-iv/), [Valid Palindrome III](https://leetcode.com/problems/valid-palindrome-iii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- How do we verify if string is a palindrome of another string?
    - We compare str[i] and str[j], and iterate i++, j- -, we can verify it is palindrome because could not find str[i] != str[j].
- Can a string become a palindrome?
    - If str is not a palindrome, but if delete a character in string, it can become palindrome.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: "aba"
Output: True

Input: s = "abc"
Output: false

EDGE CASE 
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Sort
- Two pointer solutions (left and right pointer variables): Try using two pointers technique. One pointer starts from the beginning, one starts from the end. Whenever two pointers found two unmatched characters, skip the character from either side to see if the remaining parts fits the palindrome requirement
- Storing the elements of the array in a HashMap or a Set
- Traversing the array with a sliding window

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Convert the string to array of characters, then iterate the array and in each iteration, filter out the element at current index and check if filtered array is palindrome or not. We try to check the comparison of head & tail elements of the string, if they are identical characters, check the next pair, otherwise, one of them will be the 'key' we want to find.

```markdown
1. Use two strings, one is normal and other is reversed.
2. Iterate the original string and in each iteration, remove the i'th character from the beginning in the normal string and from rear in the reversed string.
3. After removing the characters, if both the string matches that means they are palindrome.
```

⚠️ **Common Mistakes**

* The tricky part is, if the given string is not a palindrome, then we can delete at most one character from a string. After deleting a character we have to check whether a new string is a palindrome or not. The idea here is to traverse a string from both the ends using two pointers. Let’s say the two variables are left and right. The initial values of left and right are 0 and string length minus one.
Then run a loop and start comparing characters present at left and right index. If characters are equal then increment left pointer and decrement right pointer. If it is not equal then check whether a string is palindrome by deleting either the character present at left or right index.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution(object):
    def validPalindrome(self, s):
        def isPalindrome(left, right, count):
            if count > 1:
                return False
            
            while left < right:
                if s[left] != s[right]:
                    return isPalindrome(left+1, right, count+1) or isPalindrome(left, right-1, count+1)
                left += 1
                right -= 1
                
            return True
        
        return isPalindrome(0, len(s)-1, 0)
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(N) because we will run the algorithm until the left and right pointers meet.
* **Space Complexity**: O(1) because we use two pointers.
