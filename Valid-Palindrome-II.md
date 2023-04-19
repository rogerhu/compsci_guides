## Problem Highlights

* 🔗 **Leetcode Link:** [Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Recursion
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

**General Idea:**  We try to check the comparison of head & tail elements of the string, if they are identical characters, check the next pair, otherwise we recursively try the next two possibilities, skip one character from the left or skip one character from the right

```markdown
1. Write a recursive function to handle two pointer solution while maintaining the count
2. Set the basecase if the number of available skips is below 1, return false
3. Now move the start pointer to right so it points to a alphanumeric character. Similarly move end pointer to left so it also points to a alphanumeric character.
4. Now check if both the characters are same or not (ignoring cases):
- If it is not equal then we know string is not a valid palindrome, hence return two possibilities skip one character on the left or skip one or skip one character on the right. (be sure to reduce the number of available skips by 1)
- Else continue to next iteration and repeat the same process of moving both pointers to point to next alphanumeric character till start<end.
5. After loop finishes, the string is said to be palindrome, hence return true.
6. Call start and end and point them with the two ends of the input string with the number of available skips to be 1.
```

⚠️ **Common Mistakes**

* The tricky part is, if the given string is not a palindrome, then we can delete at most one character from a string. After deleting a character we have to check whether a new string is a palindrome or not. 

* The idea here is to traverse a string from both the ends using two pointers. Let’s say the two variables are left and right. The initial values of left and right are 0 and string length minus one. Then run a loop and start comparing characters present at left and right index. If characters are equal then increment left pointer and decrement right pointer. If it is not equal then check whether a string is palindrome by deleting either the character present at left or right index.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution(object):
    def validPalindrome(self, s):

        # Write a recursive function to handle two pointer solution while maintaining the count
        def isPalindrome(left, right, count):
            # Set the basecase if the number of available skips is below 1, return false
            if count > 1:
                return False
            
            # Now move the start pointer to right so it points to a alphanumeric character. 
	    # Similarly move end pointer to left so it also points to a alphanumeric character
            while left < right:

                # Now check if both the characters are same or not (ignoring cases):
                # If it is not equal then we know string is not a valid palindrome, hence return two possibilities 
                # skip one character on the left or skip one or skip one character on the right. 
		# (be sure to reduce the number of available skips by 1)
                if s[left] != s[right]:
                    return isPalindrome(left+1, right, count+1) or isPalindrome(left, right-1, count+1)
                
               # Else continue to next iteration and repeat the same process of moving both pointers to point to next alphanumeric character till start<end.
                left += 1
                right -= 1
            
	    # After loop finishes, the string is said to be palindrome, hence return true.
            return True

        # Call start and end and point them with the two ends of the input string with the number of available skips to be 1.                       
        return isPalindrome(0, len(s)-1, 0)
```
```java

class Solution {
    public boolean validPalindrome(String s) {
        // Call start and end and point them with the two ends of the input string with the number of available skips to be 1. 
        return isPalindrome(s, 0, s.length()-1, 1);
    }
    // Write a recursive function to handle two pointer solution while maintaining the count
    public boolean isPalindrome(String str, int start, int end, int chance) {
        // Set the basecase 
        // if the start and end cross, return true
            if(start >= end) return true;

            // Now move the start pointer to right so it points to a alphanumeric character. 
            //  Similarly move end pointer to left so it also points to a alphanumeric character
            if(str.charAt(start) == str.charAt(end))
                return isPalindrome(str, start+1, end-1, chance);
            
            // if the number of available skips is below 1, return false
            if(chance == 0) return false;
            
            // If it is not equal then we know string is not a valid palindrome, hence return two possibilities 
            // skip one character on the left or skip one or skip one character on the right. 
            // (be sure to reduce the number of available skips by 1)
            return isPalindrome(str, start+1, end, chance-1)  || isPalindrome(str, start, end - 1,chance-1);
    }
}
```    

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(N) because we will run the algorithm at most twice due to a single skip. (N being the number of characters in the string) 
    - However, if we had more number of skips like 4, then we will have O(N * 2^M) M being the number of skips available, because each time we skip we will have to double the number of possible strings to check. 
    - Here, we have O(N * 2^1), which is O(N*2), which is basically O(N).
* **Space Complexity**: O(1) because we use two pointers.
