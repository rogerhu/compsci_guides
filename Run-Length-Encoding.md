## Problem Highlights

* 🔗 **Geeksforgeeks Link:**  [Run Length Encoding](https://www.geeksforgeeks.org/run-length-encoding/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Array
* 🗒️ **Similar Questions**: [String Compression](https://leetcode.com/problems/string-compression/), [Count and Say](https://leetcode.com/problems/count-and-say/)
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How should we encode letters that are the same, but do not appear next to each other?
    - We only want to encode consecutive letters that are the same. If identical letters are not next to each other, encode them separately.

- Can we expect an empty input string? What do we return in that case?
    - Assume an empty string is a valid input. In this case, return an empty string.

- Are there any space or time constraints?
    - Try solving this with O(1) space.

```markdown
HAPPY CASE
Input: "aabaccccdexdx"
Output: "a2b1a1c4d1e1x1d1x1"

Input: "wwwwaaadexxxxxx"
Output: “w4a3d1e1x6”
 
 
EDGE CASE
Input: ""
Output: ""
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Strings/Arrays, some things we should consider are:

- Sort. 
    - Sorting isn't very effective for counting here.. 
- Two pointer solutions (left and right pointer variables). 
    - Two pointer solution doesn't help us count the frequency of the characters.
- Storing the elements of the array in a HashMap or a Set. 
    - Can we store any useful information about the elements in the string? (e.g. number of occurrences)    
- Traversing the array with a sliding window. 
    - Can we capture pieces of the string and calculate anything about them using a sliding window?



## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Look through each element in the array and increment a counter every time a new character is found. Print the count and character to the new string.

```markdown
1) Pick the first character from the source string. 
2) Append the picked character to the destination string. 
3) Count the number of subsequent occurrences of the picked character and append the count to the destination string. 
4) Pick the next character and repeat steps b) c) and d) if the end of the string is NOT reached.
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def printRLE(st):

 n = len(st)
 i = 0
 while i < n- 1:

  # Count occurrences of
  # current character
  count = 1
  while (i < n - 1 and
   st[i] == st[i + 1]):
   count += 1
   i += 1
  i += 1

  # Print character and its count
  print(st[i - 1] +
   str(count),
   end = "")

# Driver code
if __name__ == "__main__":

 st = "wwwwaaadexxxxxxywww"
 printRLE(st)

# This code is contributed by Chitranayal
```
```java
// Java program to implement run length encoding

public class RunLength_Encoding {
 public static void printRLE(String str)
 {
  int n = str.length();
  for (int i = 0; i < n; i++) {

   // Count occurrences of current character
   int count = 1;
   while (i < n - 1 &&
    str.charAt(i) == str.charAt(i + 1)) {
    count++;
    i++;
   }

   // Print character and its count
   System.out.print(str.charAt(i));
   System.out.print(count);
  }
 }

 public static void main(String[] args)
 {
  String str = "wwwwaaadexxxxxxywww";
  printRLE(str);
 }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of elements in the array.

* **Time Complexity**: `O(N)` because we will need to access each character in the word.
* **Space Complexity**: `O(1)` because we are printing the string encoding