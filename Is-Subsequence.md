# Problem

Write a function that takes in two strings and returns true if the second string is substring of the first, and false otherwise.

## Problem Highlights

* 🔗 **Leetcode Link:** [Is Subsequence](https://leetcode.com/problems/is-subsequence/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Strings, Two Pointer
* 🗒️ **Similar Questions**: [Number of Matching Subsequences](https://leetcode.com/problems/number-of-matching-subsequences/), [Shortest Way to Form String](https://leetcode.com/problems/shortest-way-to-form-string/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Are both inputs only strings?
    - Yes, both inputs are only strings
- Can elements of the string be both characters and numbers?
    - yes any character/number is valid.
- Can the second string be larger than the first?
    - yes, the second string can be larger than the first.
- What are the Time & Space considerations?
  - Best Case Time Complexity: `O(n)` if n is the length of t. 
  - Best Case Space Complexity: `O(1)`


Run through a set of example cases:

```markdown
HAPPY CASE
Input: s = "abc", t = "ahbgdc"
Output: true

Input: s = "axc", t = "ahbgdc"
Output: false

EDGE CASE
Input: "", ""
Output: true
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Strings, common solution patterns include:

- Sort
    - Does sorting help us achieve what we need in order to solve the problem?
- Two pointer solutions (left and right pointer variables)
    - Two pointer may help us, if we start at the beginning of each string, we can check if we can match each character in string 's' while we progress through string 't'. Verifying if 's' is a subsequence of 't'.
- Storing the elements of the array in a HashMap or a Set
    - A hashset will be not helpful here.
- Traversing the array with a sliding window
    - Will viewing pieces of the input at a time help us?


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Identify each `s` character in string t in relative order. We will progress through string 's' if it exist in linear order in string 't'. If we get to the end of string 's', we verified that 's' is substring of 't', otherwise it is not. 

```markdown
1) Create two pointers to represent the character progression of string 's' and 't'
2) While pointerS is less than length of string 's' and pointerT is less than length of string 't'
    a) Let's progress the pointerS every time we match with pointerT.
    b) Let's progress pointerT everytime so we can get the next character. 
3) Return whether or not we reached the end of string 's' by matching pointerS to length of string 's'.
```

⚠️ **Common Mistakes**

* Some students may want to hash components of the strings, but this does not preserve order.
* We can solve this problem without any extra allocated space.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        # Create two pointers to represent the character progression of string 's' and 't'
        pointerS = pointerT = 0

        # While pointerS is less than length of string 's' and pointerT is less than length of string 't'
        while pointerS < len(s) and pointerT < len(t):

            # Let's progress the pointerS every time we match with pointerT
            if s[pointerS] == t[pointerT]:
                pointerS += 1

            # Let's progress pointerT everytime so we can get the next character
            pointerT += 1
        
        # Return whether or not we reached the end of string 's' by matching pointerS to length of string 's'
        return pointerS == len(s)
```
```java
class Solution {
    public boolean isSubsequence(String s, String t) {
        // Create two pointers to represent the character progression of string 's' and 't'
        int i = 0, j = 0;

        // While pointerS is less than length of string 's' and pointerT is less than length of string 't'
        while(i < s.length() && j < t.length()){
            // Let's progress the pointerS every time we match with pointerT
            if(s.charAt(i) == t.charAt(j)) i++;

            // Let's progress pointerT everytime so we can get the next character
            j++;
        }

        // Return whether or not we reached the end of string 's' by matching pointerS to length of string 's'
        return i == s.length();
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
Assume `N` represents the number of characters in the string 's' and `M` represents the number of characters in the string 't'.

* **Time Complexity**: `O(M)`, because we only need to progress through all the characters in string 't' to verify if string 's' is a subsequence of string 't'.
* **Space Complexity**: `O(1)`, because we used two pointers which has a constant space cost. 