## Problem Highlights

* 🔗 **Leetcode Link:** [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) 
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Strings, Two Pointer
* 🗒️ **Similar Questions**: [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/), [Maximum Product of the Length of Two Palindromic Subsequences](https://leetcode.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences/), [Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Do we only consider the alphabet and numbers?
  - In this problem, we ignore all the characters except alphabets and numbers.
- How can we check if string is a palindrome without creating extra memory?
  - We can also use two pointers for checking if it is a palindrome or not and we need not to filter or save it by creating extra memory. Having two pointer at the ends of the string, we can compare character by character.
   
```markdown
HAPPY CASE
Input: s = "A man, a plan, a canal: Panama"
Output: true

Input: s = "race a car"
Output: false

EDGE CASE
Input: s = " "
Output: true
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Sort
- Two pointer solutions (left and right pointer variables): What we can do is take two pointer variables: start and end pointers. Then, point them with the two ends of the input string. When left pointer and right pointer pointing at different valid character, we should exit and return false immediately.
- Storing the elements of the array in a HashMap or a Set
- Traversing the array with a sliding window

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Start traversing inwards, from both ends of the input string, and we can expect to see the same characters, in the same order.

```markdown
1. What we can do is take two pointer variables, start and end and point them with the two ends of the input string.
2. Now move the start pointer to right so it points to a alphanumeric character. Similarly move end pointer to left so it also points to a alphanumeric character.
3. Now check if both the characters are same or not (ignoring cases):
If it is not equal then we know string is not a valid palindrome, hence return false.
Else continue to next iteration and repeat the same process of moving both pointers to point to next alphanumeric character till start<end.
4. After loop finishes, the string is said to be palindrome, hence return true.
```

⚠️ **Common Mistakes**

* A less optimized approached would be to check if a string is palindrome or not we can simply reverse it and compare it with the original string. After reversing if it remains equal then the given string is a palindrome. We will need O(n) additional space to store the filtered string and the reversed string.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def isPalindrome(self, s: str) -> bool:
        i = 0
        while i < len(s):
            while i < len (s) and not s[i].isalnum():
                s = s.replace(s[i],"") #delete all non alphanumeric characters
            i+=1
        s, j = s.lower(), len(s)-1  #convert s to lower
        for i in range(len(s)//2): #i goes from first to middle character, j goes from last to middle character
            if s[i] != s[j]:
                return False
            j-=1
        return True
```
```java
public boolean isPalindrome(String s) {
        // create pointers to the front and back of the string
        int back = s.length()-1;
        int front = 0;
        
        // transform the string to upperCase
        s = s.toUpperCase();
        
        // while front pointer != back pointer
        while (front < back){
            // move until we find a alphanumeric character
            while (!Character.isLetterOrDigit(s.charAt(front)) && front < back){
                front++;
            }
            while (!Character.isLetterOrDigit(s.charAt(back)) && back > front){
                back--;
            }
            // make sure they are the same
            if (s.charAt(front) != s.charAt(back)){
                return false;
            }
            back--;
            front++;
        }
        return true;
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(n), traversing is done for almost every character, until the pointers meet in the middle or if the required condition is not true we return early. Time complexity will be O(n) (where n is the length of the given string).
* **Space Complexity**: O(1), we do not need any extra memory here.