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

- What are the constraints?
  - 1 <= maxChoosableInteger <= 20, 0 <= desiredTotal <= 300

- What is the state of the game?
  - Intuitively, to uniquely determine the result of any state, we need to know: the unchosen numbers and the remaining desiredTotal to reach. Both are related as we can always get the remaining desiredTotal by deducting the sum of chosen numbers from original desiredTotal. 

- How many allowed values sets are possible? 
  - The length of the allowed value set can range 1 to maxChoosableInteger(N). So the answer is (N,1) + (N,2) + ..(N,N) where (N,K) means choose K from N. This is equal to 2^N.


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

The strategy is to avoid repeatedly solving sub-problems. That is a clue to use memoization. Memoization is a way to potentially make functions that use recursion run faster.  How we define the state of the game determines how we will do memoization as well. Clearly list of current allowed numbers are needed to define the state. Therefore only allowed values uniquely determine the state. Memoizing allows us to store input alongside the result of the calculation. Therefore, rather than having to do the same work again using the same input, it can simply return the value stored in the cache.

**⚠️ Common Mistakes**

* Without memoization, we cannot create a cache where we store inputs with their calculated results. Whenever we have an input that we’ve already seen, we can simply retrieve the result rather than redoing any of our work. By creating a cache, we’re using additional space, so you have to decide whether that’s worth the improved speed. If you have a very large range of inputs where it’s fairly unlikely that you’ll need to repeat the same calculations, memoization may not be an efficient solution.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.


```markdown
We create an array allowed which has all the integers from 1 to maxChoosableInteger.
At each state, if the maximum usable number plus the numbers you already summed up (denote as already) is greater than the desired total, it means you can definitely win at this state.
Else, you loop through the usable numbers, each time pick one number, and see whether your opponent can win at the new state. If he can't, then you can win. (Really, I don't see the logic here, hope someone can explain. Isn't there a situation where none of us can definitely win?) And remember to cache the result.
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution(object):
    def canIWin(self, maxChoosableInteger, desiredTotal):
        from math import *
        usable = [x for x in range(1,maxChoosableInteger +1)]
        if (1+ maxChoosableInteger) * maxChoosableInteger <= desiredTotal:
            return False
        else:
            return self.hint(maxChoosableInteger,desiredTotal,usable,{},0)
        
    def hint(self,maxChoosbleInt,desiredTotal,usable,cache,already):
        if len(usable) == 0:
            return False
        else:
            state = tuple(usable)
            if state in cache:
                return cache[state]
            else:
                cache[state] = False
                if max(usable) + already >= desiredTotal:
                    cache[state] = True
                elif len(usable) >1 and max(usable) + already >= desiredTotal:
                    cache[state] = False
                else:
                    for x in usable:
                        newstate = [y for y in usable if y!= x]
                        if not self.hint(maxChoosbleInt,desiredTotal,newstate,cache,already + x):
                            cache[state] = True
                            break
                return cache[state]   
```

    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Given n is maxChoosableInteger,

* **Time Complexity**: O(2^n), for any given state of choices, the remainder must be the same since there is a relationship between the two variable of `remainder = summed_choices - sum(choices)`, all of which are constant.

* **Space Complexity**: O(n)