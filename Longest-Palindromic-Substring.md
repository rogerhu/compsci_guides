## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/longest-palindromic-substring/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Dynamic Programming
* 🗒️ **Similar Questions**: [Palindrome Permutation](https://leetcode.com/problems/palindrome-permutation/), [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input be empty or Null?
  - The input could be empty but not Null.
- What is a palindrome? 
  - A palindrome is a string which reads the same in both directions. For example, S = "aba" is a palindrome, S = "abc" is not.   

Run through a set of example cases:

```markdown
HAPPY CASE
Input: "babad"
Output: "bab", NOTE: "aba" is also another valid answer

Input: "cbbd"
Output: "bb"

EDGE CASE (Empty String)
Input: ""
Output: ""
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For graph problems, we want to consider the following approaches:

* Dynamic Programming Approach: The time complexity can be reduced by storing results of subproblems. The next time the same palindromic substring is spotted, instead of recomputing its solution, one simply looks up the previously computed solution, thereby saving computation time at the expense of a modest expenditure in storage space.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** 2-D DP Array to help store previous solutions. Grow solution by iterating through the 2D Array and comparing characters that are added to previous palindromes.

```markdown
1) Create a 2D Boolean Array of N x N size
2) Iterate through the array in column-major order, variable r (row)
    a) Nested iteration with upper bound of r, variable c (column)
        i) If the elements at indices r and c of the input string are identical, 
            and the difference is at max 2, this is a sub-palindrome. 
            Mark DP[r][c] as True.
        ii) If the elements at indices r and c are identical yet the difference 
            is greater than 2, refer to the sub-problem of DP[r + 1][c - 1] 
            and build on top of previous solution.
            - DP[r + 1][c - 1] refers to the sub-string 1 element smaller on both 
                sides when compared to the current slice from r -> c.
3) Perform another nested iteration, identical pattern as before.
    a) Identify the largest length substring (c - r + 1) that is a palindrome 
        (DP[r][c] is True)
4) Return the substring slice of the largest substring found in the second nested traversal.

```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

It is easy to get confused between the terms substring and subsequence. Be sure to Verify that you understand the definition of this problem.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        # Dynamic Programming
        # P(i,j) means if the substring s_i, .... , s_j is a palindrome
        if len(s) <= 1:
            return s
        
        # Initialize a memoization P 
        P = [[None for _ in range(len(s))] for _ in range(len(s))]
        
        # Populate known values
        # One letter palindrome p(i,i)
        for i in range(len(s)):
            P[i][i] = True
        
        # Bottom up
        # P(i,j) = P(i+1,j-1) && s[i] == s[j]
        for j in range(1, len(s)):
            for i in range(0,j):
                if P[i][j] is None:
                    
                    # find cond = P(i+1,j-1)
                    in_i = i+1
                    in_j = j-1
                    cond = False
                    if in_i >= in_j:
                        # represent one character in the middle substring xAy
                        # or empty string xy
                        cond = True # basically ignore the condition
                    else:
                        cond = P[in_i][in_j]
                    
                    P[i][j] = cond and s[i] == s[j]
            
        # for ps in P:
        #     print(ps)
            
        # Get result
        # Longest palindrome will have max distance (i, j)
        max_dist = -1 # 0 will mean one character (i, i)
        max_ij = None
        for i in range(0,len(s)):
            for j in reversed(range(0,len(s))):
                if P[i][j] and (j-i) > max_dist:
                    max_dist = j-i
                    max_ij = (i, j)
        if max_ij: 
            return s[max_ij[0]:max_ij[1]+1]
        else:
            return None
```
```java
public String longestPalindrome(String s) {
        int length = s.length();
        if (length < 2) {
            return s;
        }

        // table[i][j] == true <=> s.substring(i, j + 1) is a palindrome
        boolean[][] table = new boolean[length][length];

        // these variables will store the indices of the max palindrome
        int beginIndex = 0;
        int endIndex = 0;

        for (int left = length - 1; left >= 0 ; left--) {
            for (int right = left; right < length; right++) {
                if (s.charAt(left) == s.charAt(right) && (right - left < 3 || table[left + 1][right - 1])) {
                    table[left][right] = true;
                    if (right - left > endIndex - beginIndex - 1) {
                        beginIndex = left;
                        endIndex = right + 1;
                    }
                }
            }
        }

        return s.substring(beginIndex, endIndex);
    }
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(n^2), 
* **Space Complexity**: O(n^2) space to store the table.