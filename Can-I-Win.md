## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/can-i-win/](https://leetcode.com/problems/can-i-win/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Recursion, Memoization
* 🗒️ **Similar Questions**: [Flip Game II](https://leetcode.com/problems/flip-game-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- 

- 


```markdown
Example 1:

Input: maxChoosableInteger = 10, desiredTotal = 11
Output: false
Explanation:
No matter which integer the first player choose, the first player will lose.
The first player can choose an integer from 1 up to 10.
If the first player choose 1, the second player can only choose integers from 2 up to 10.
The second player will win by choosing 10 and get a total = 11, which is >= desiredTotal.
Same with other integers chosen by the first player, the second player will always win.

Example 2:

Input: maxChoosableInteger = 10, desiredTotal = 0
Output: true

Example 3:

Input: maxChoosableInteger = 10, desiredTotal = 1
Output: true
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

The strategy is to avoid repeatedly solving sub-problems. That is a clue to use memoization. Memoization is a way to potentially make functions that use recursion run faster. A recursive function might end up performing the same calculation with the same input multiple times. Memoizing allows us to store input alongside the result of the calculation. Therefore, rather than having to do the same work again using the same input, it can simply return the value stored in the cache.

**⚠️ Common Mistakes**

* Without memoization, we cannot create a cache where we store inputs with their calculated results. Whenever we have an input that we’ve already seen, we can simply retrieve the result rather than redoing any of our work. By creating a cache, we’re using additional space, so you have to decide whether that’s worth the improved speed. If you have a very large range of inputs where it’s fairly unlikely that you’ll need to repeat the same calculations, memoization may not be an efficient solution.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** 


```markdown

```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def canIWin(self, maxChoosableInteger: int, desiredTotal: int) -> bool:
        
        def check(nums, t):
            if nums[-1] >= t: return True
            if tuple(nums) in memo: return memo[tuple(nums)]
            res = any(not check(nums[:i]+nums[i+1:],t-nums[i]) for i in range(len(nums)))
            return memo.setdefault(tuple(nums), res)
            
        if maxChoosableInteger * (1 + maxChoosableInteger) // 2 < desiredTotal: return False
        memo, nums = {}, list(range(1, maxChoosableInteger + 1))
        return check(nums, desiredTotal)       
```

    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(2^k), for any given state of choices, the remainder must be the same since there is a relationship between the two variable of `remainder = summed_choices - sum(choices)`, all of which are constant.

* **Space Complexity**: 