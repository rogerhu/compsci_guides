## Problem Highlights

* 🔗 **Leetcode Link:** [Shortest Distance to a Character](https://leetcode.com/problems/shortest-distance-to-a-character/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Recursion
* 🗒️ **Similar Questions**: [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/), [Valid Palindrome IV](https://leetcode.com/problems/valid-palindrome-iv/), [Check Distances Between Same Letters](https://leetcode.com/problems/check-distances-between-same-letters/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- How do we verify the distance away from a character?
    - We can check array for distance away from a character. 
- What happens if a sting is of length 0?
    - All strings will be 1 character or longer
- What happens if c is not found in s?
    - c is guaranteed to occur at least once in s
- What are the time and space constraints?
    - Time should be `O(N)` and space should be `O(N)`, `N` being the length of the string.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: s = "loveleetcode", c = "e"
Output: [3,2,1,0,1,0,0,1,2,2,1,0]
Explanation: The character 'e' appears at indices 3, 5, 6, and 11 (0-indexed).
The closest occurrence of 'e' for index 0 is at index 3, so the distance is abs(0 - 3) = 3.
The closest occurrence of 'e' for index 1 is at index 3, so the distance is abs(1 - 3) = 2.
For index 4, there is a tie between the 'e' at index 3 and the 'e' at index 5, but the distance is still the same: abs(4 - 3) == abs(4 - 5) = 1.
The closest occurrence of 'e' for index 8 is at index 6, so the distance is abs(8 - 6) = 2.

Input: s = "aaab", c = "b"
Output: [3,2,1,0]

EDGE CASE 
Input: s = "b", c = "b"
Output: [0]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Sort 
    - Not helpful, we need to maintain the relative order of the string
- Two pointer solutions (left and right pointer variables)
    - This is a viable technique. We move from the point where c occurs in s, in both directions.
- Storing the elements of the array in a HashMap or a Set
    - Not helpful, how does the hashmap help us find the distance between characters
- Traversing the array with a sliding window
    - We will have aspects of this technique in our solution to break our loop/recursion

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:**  We check the entire string for c. Upon finding c we will seek both ways and update distance away from c as long as the previous distance away from c was smaller. 

```markdown
1. Write a recursive function to handle two pointer solution while updating the distance.
    a. Set the basecase: Out of bound or previous distance away from c was smaller.
    b. Set the distance for index and recursively call left and right of index.
2. Call the recursive function at each point where we find c in string.
3. Return the result.
```

⚠️ **Common Mistakes**

* Using iterative approach will work here, however will make the code a bit messier and makes the logic harder to express.  

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def shortestToChar(self, s: str, c: str) -> List[int]:
      # Write a recursive function to handle two pointer solution while updating the distance.
      def helper(i, distance):
        # Set the basecase: Out of bound or previous distance away from c was smaller.
        if i < 0 or i > len(s) - 1 or res[i] <= distance:
          return 

        # Set the distance for index and recursively call left and right of index.
        res[i] = distance
        helper(i + 1, distance + 1)
        helper(i - 1, distance + 1)
      
      # Call the recursive function at each point where we find c in string
      res = [len(s) for i in range(len(s))]
      for i, char in enumerate(s):
        if char == c:
          helper(i, 0)
      
      # Return the result
      return res
```
```java
class Solution {
    public int[] shortestToChar(String S, char C) {
        int len = S.length();
        int[] dist = new int[len];
        Arrays.fill(dist, len);
        
        // Call the recursive function at each point where we find c in string
        for(int i = 0; i < len; i++) {
            if(S.charAt(i) == C) {
                minimizeSurroundings(S, dist, i, 0);
            }
        }
        
        // Return the result
        return dist;
    }
    
    // Write a recursive function to handle two pointer solution while updating the distance
    private void minimizeSurroundings(String S, int[] dist, int idx, int cur) {
      // Set the basecase: Out of bound or previous distance away from c was smaller
      if(idx < 0 || idx >= S.length() || dist[idx] <= cur)
          return;
      
      // Set the distance for index and recursively call left and right of index
      dist[idx] = cur;
      minimizeSurroundings(S, dist, idx-1, cur+1);
      minimizeSurroundings(S, dist, idx+1, cur+1);
    }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
Assume `N` represents the length of the s

* **Time Complexity**: O(N) because we will run the algorithm at most twice because recursive call will at most read the entire array once, due to early exit when smaller distance away from c is found. 
* **Space Complexity**: O(N) because we need to store results the length of s. Also recursive call maybe the length of the entire string s. 
