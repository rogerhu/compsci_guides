## Problem Highlights

* 🔗 **Leetcode Link:** [Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Strings
* 🗒️ **Similar Questions**: [Reverse String](https://leetcode.com/problems/reverse-string/), [Reverse String II](https://leetcode.com/problems/reverse-string-ii/), [Reverse Words in a String III](https://leetcode.com/problems/reverse-words-in-a-string-iii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could the input parameter be Null?
  - Let’s assume no input will be Null. However, the input could be an empty string.

```markdown
HAPPY CASE
Input: "the sky is blue"
Output: "blue is sky the"

Input: "what is the time"
Output: "time the is what"

EDGE CASE (Multiple Spaces)
Input: "the   sky  is   blue"
Output: "blue is sky the"
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Sort
    - Does sorting help us achieve what we need in order to solve the problem?
- Two pointer solutions (left and right pointer variables)
    - Two pointer may help us tokenize the object if we are not allowed to use built-in language methods.
- Storing the elements of the array in a HashMap or a Set
    - In reversing, hashing elements and storing them may not yield an optimal solution.
- Traversing the array with a sliding window
    - Will viewing pieces of the input at a time help us?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Tokenize the input string into words separated by spaces and rejoin the tokens in reversed order.

```markdown
1) Tokenize the input string to create a separate array 
2) Return a joined string version of the reversed array
```

⚠️ **Common Mistakes**

* You may have a hard time understanding this problem because most questions are the standard reverse a string with a single word problem.
* Some people get stuck on solving the multiple spaces sub-problem.

## 4: I-mplement

> **Implement** the code to solve the algorithm.


```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # Tokenize the input string to create a separate array 
        arr = []
        temp = ""
        for c in s:
            if c != " ":
                temp += c 
            elif temp != "":
                arr.append(temp)
                temp = ""
        if temp != "":
            arr.append(temp)

        # Return a joined string version of the reversed array
        l, r = 0, len(arr) - 1
        while l<r:
            arr[l], arr[r] = arr[r], arr[l]
            l += 1
            r -= 1
        return " ".join(arr)
```
```java
class Solution {
    public String reverseWords(String s) {
        // Tokenize the input string to create a separate array
        ArrayList<String> arr = new ArrayList<>();
        String temp = "";
        for (char c : s.toCharArray()) {
            if (c != ' ') {
                temp += c;
            } else if (!temp.isEmpty()) {
                arr.add(temp);
                temp = "";
            }
        }
        if (!temp.isEmpty()) {
            arr.add(temp);
        }

        // Return a joined string version of the reversed array
        int l = 0, r = arr.size() - 1;
        while (l < r) {
            String swap = arr.get(l);
            arr.set(l, arr.get(r));
            arr.set(r, swap);
            l++;
            r--;
        }
        return String.join(" ", arr);
    }
}
```

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # Tokenize the input string to create a separate array 
        arr = [token for token in s.split() if token != ""]
        
        # Return a joined string version of the reversed array
        return " ".join(reversed(arr))
```
```java
class Solution {
    public String reverseWords(String s) {
        StringBuilder sb = new StringBuilder();
        // Tokenize the input string to create a separate array 
        String[] array = s.split(" ");

        // Return a joined string version of the reversed array
        for (int i = array.length - 1; i >= 0; i--) {
            if (!array[i].isEmpty()) {
                sb.append(array[i]);
                sb.append(" ");
            }
        }

        return sb.toString().trim();
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of character in the string.

* **Time Complexity**: O(N), traversing done on every word in string
* **Space Complexity**: O(N), we will be building a string with the length of the string in reverse.