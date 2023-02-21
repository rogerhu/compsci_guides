# Problem

Write a function that takes in two strings and returns true if the second string is substring of the first, and false otherwise.

## Problem Highlights

* 🔗 **Leetcode Link:** [Substring](https://leetcode.com/problems/is-subsequence/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Strings, Hash
* 🗒️ **Similar Questions**: [Number of Matching Subsequences](https://leetcode.com/problems/number-of-matching-subsequences/), [Shortest Way to Form String](https://leetcode.com/problems/shortest-way-to-form-string/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Are both inputs only strings?
- Can elements of the string be both characters and numbers?
- Can the second string be larger than the first?
- What are the Time & Space considerations?
  - Best Case Time Complexity: O(n * m)
  - Best Case Space Complexity: O(1)


Run through a set of example cases:

```markdown
HAPPY CASE
Input: laboratory, rat
Output: true

Input: cat, meow
Output: false

EDGE CASE
Input: "CATDOG", ""
Output: True
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Strings, common solution patterns include:

- Two pointer solutions (left and right pointer variables) or a pointer on each string
- Storing the characters of the string in a HashMap or a Set
- Traversing the string with a sliding window


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Loop through the first string for an initial match. If we find one, check that match for a full substring match.

```markdown
1) Edge Check
2) Loop through the first string
3) Find a match to the first character of the second string
4) While there is a match, loop through both strings in parallel to ensure 
    the match
5) If there is an entire match to the second string, return True (there is a 
    substring)
6) If the match breaks, continue searching the first string for an initial 
    character match
7) If we make it to the end of the first string without a full match, there 
    is no substring match -- return False 
```

⚠️ **Common Mistakes**

* Some students may want to hash components of the strings, but this does not preserve order.
* We can solve this problem without any extra allocated space.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def substring(large_str, potential_substr):
    substr_len = len(potential_substr)

    # iterate through larger str for its length minus substr length
    for i in range(len(large_str) - substr_len):
        # iterate through potential substr  
        for j in range(substr_len):
            # if chars match in both string
            if large_str[i + j] == potential_substr[j]: 
                # if at the end of substring, return true
                if j == (substr_len - 1):
                    return True
                # else do nothing and continue looping
            else:
                break

    # did not find substring
    return False
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: `O(n*m)`, where `n`: length of first string, `m`: length of second string
* **Space Complexity**: O(1), where no data structures are needed in this problem