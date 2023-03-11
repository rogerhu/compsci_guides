## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/house-robber/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Dynamic Programming
* 🗒️ **Similar Questions**: [House Robber II](https://leetcode.com/problems/house-robber-ii/), [House Robber III](https://leetcode.com/problems/house-robber-iii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can you rob all the houses on the street?
  - They can rob any of the houses on the street...if there were no alarms in the houses. 
- Is there a greedy way of making choices in which the overall profit is maximized?
  - There is no greedy way of deciding if the robber should rob a house or not. 
- Can a robber come back and rob the same house they rejected?
  - Technically, a robber can come back and rob a house that they previously rejected. However, since we are trying all options, we will not go back and rob an unrobbed house since that scenario will be covered in a different recursive path.
   
```markdown
EXAMPLE CASE
Input: nums = [1,2,3,1]
Output: 4

Input: nums = [2,7,9,3,1]
Output: 12
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

The dynamic programming approach can be used here. Every DP solution has a table that we populate starting with the base case or the simplest of cases for which we already know the answer. E.g. for our problem, we know that in the absence of houses, the robber will make 0 profit. Similarly, if there is just one house left to rob, the robber will rob that house, and that will be the maximum profit.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

We start by populating the dynamic programming table with these initial values and then build the table in a bottom-up fashion which is the essence of this solution.

```markdown
1. Define a table which we will use to store the results of our sub-problems. Call this table maxRobbedAmount where maxRobbedAmount[i] is the same value that would be returned by recurse(i) in the previous solution
2. Set maxRobbedAmount[N] to 0 
3. Set maxRobbedAmount[N - 1] to nums[N - 1]
4. Iterate from N - 2 down to 0 and set maxRobbedAmount[i] = max(maxRobbedAmount[i + 1], maxRobbedAmount[i + 2] + nums[i])
5. Return the value in maxRobbedAmount[0].

```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

The recursive approach may run into trouble when the recursion stack grows too large. It may also run into trouble because, for each recursive call, the compiler must do additional work to maintain the call stack (function variables, etc.) which results in unwanted overhead.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        
        # Special handling for empty case.
        if not nums:
            return 0
        
        maxRobbedAmount = [None for _ in range(len(nums) + 1)]
        N = len(nums)
        
        # Base case initialization.
        maxRobbedAmount[N], maxRobbedAmount[N - 1] = 0, nums[N - 1]
        
        # DP table calculations.
        for i in range(N - 2, -1, -1):
            
            # Same as recursive solution.
            maxRobbedAmount[i] = max(maxRobbedAmount[i + 1], maxRobbedAmount[i + 2] + nums[i])
            
        return maxRobbedAmount[0]    
```
```java
class Solution {
    
    public int rob(int[] nums) {
        
        int N = nums.length;
        
        // Special handling for empty array case.
        if (N == 0) {
            return 0;
        }
        
        int[] maxRobbedAmount = new int[nums.length + 1];
        
        // Base case initializations.
        maxRobbedAmount[N] = 0;
        maxRobbedAmount[N - 1] = nums[N - 1];
        
        // DP table calculations.
        for (int i = N - 2; i >= 0; --i) {
            
            // Same as the recursive solution.
            maxRobbedAmount[i] = Math.max(maxRobbedAmount[i + 1], maxRobbedAmount[i + 2] + nums[i]);
        }
        
        return maxRobbedAmount[0];
    }
}      
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(N) since we have a loop from N−2...0
* **Space Complexity**: O(N) which is used by the table