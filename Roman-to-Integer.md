## Problem Highlights

* 🔗 **Leetcode Link:** [Roman to Integer](https://leetcode.com/problems/roman-to-integer/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: String, Hash, Math
* 🗒️ **Similar Questions**: [Integer to Roman](https://leetcode.com/problems/integer-to-roman/), [Integer to English Words](https://leetcode.com/problems/integer-to-english-words/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Is it easier to iterate from right to left or left to right?
  - Subtractive numerals is that they appear before a larger number, which means they are easier to identify if we iterate from right to left than left to right.
- How can a dictionary help here?
  - A dictionary can map the basic roman numeral to its appropriate values 
   
```markdown
HAPPY CASE
Input: s = "III"
Output: 3
Explanation: III = 3.

Input: s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.

EDGE CASE
Input: MMMDCCCLXXXVIII
Output:3888
Explanation: Longest Roman Numeral
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:
* Sort. Sorting does not help solve this specific problem, because the ordering of the letters is important.
* Two pointer solutions (left and right pointer variables).Two pointer approach does not help solve this specific problem, because ordering of the letters is unidirectional.
* Storing the elements of the array in a HashMap or a Set. This could potentially work if know exactly what to look for after we store elements. Create dictionary where we are going to store the value and the index of each list element as a key-pair respectively. Then we iterate through the indices and values of the list containing our numbers. If the difference between the target and the current value in the list is already included as a key in the dictionary, then it means that the current value and the value stored in the dictionary is the solution to our problem.
* Traversing the array with a sliding window. A solution may not be two adjacent numbers, so a fixed sliding window won’t solve the problem in all cases.




## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use a dictionary to convert Roman Symbols to corresponding integers. 

```markdown
1) Maintain a map/dictionary with Roman symbols and their corresponding integer equivalent.
2) Scan the string from right to left. Get the value corresponding to the current character from the map/dictionary and add it to the result.
3) The special case is where there is a character at left of current character whose value is less than the value corresponding to the current character.  In this case, we will subtract the value of the character in the left from the result.
```

**⚠️ Common Mistakes**

* A common mistake would be complicating a solution with multiple nested condtions. Another mistake can occur when an item doesn't exist in the map, and you get a null pointer exception.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def romanToInt(s: str) -> int:
    # Dictionary of roman numerals
    roman_map = {'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000}
    # Length of the given string
    n = len(s)
    # This variable will store result
    num = roman_map[s[n - 1]]
    # Loop for each character from right to left
    for i in range(n - 2, -1, -1):
        # Check if the character at right of current character is bigger or smaller
        if roman_map[s[i]] >= roman_map[s[i + 1]]:
            num += roman_map[s[i]]
        else:
            num -= roman_map[s[i]]
    return num
```
```java

```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of letters in string.

* **Time Complexity**: `O(1)` because the maximum length of the string can be 15, therefore, the worst case time complexity can be O(15) or O(1).

* **Space Complexity**: `O(1)` because we are using map/dictionary to store the Roman symbols and their corresponding integer values but there are only 7 symbols hence the worst case space complexity can be O{7} which is equivalent to O(1).