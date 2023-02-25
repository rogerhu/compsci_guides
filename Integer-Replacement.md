## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/integer-replacement/](https://leetcode.com/problems/integer-replacement/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Recursion, Memoization
* 🗒️ **Similar Questions**: TBD
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What are the constraints?
  - 1 <= n <= 231 - 1

- How can we handle odd numbers?
  - There is subtle trick in this problem that allows us to choose either x-1 or x+1 when x is odd. Say x is 7. Then x-1 = 6 and x+1 = 8. 6/2 = 3 and 8/2=4. Now this is consistent - either (x-1)/2 is even or (x+1)/2 is even. And we want to pick the path that leads us to an even number to reach our goal fastest. One exception is x = 3 where both (x-1)/2 is odd or (x+1)/2 is even. For this case, use x-1 which takes us to 2.


```markdown
Example 1:

Input: n = 8
Output: 3
Explanation: 8 -> 4 -> 2 -> 1

Example 2:

Input: n = 7
Output: 4
Explanation: 7 -> 8 -> 4 -> 2 -> 1
or 7 -> 6 -> 3 -> 2 -> 1

Example 3:

Input: n = 4
Output: 2

```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.


The strategy is to avoid repeatedly solving sub-problems. That is a clue to use memoization. Memoization is a way to potentially make functions that use recursion run faster. A recursive function might end up performing the same calculation with the same input multiple times. Memoizing allows us to store input alongside the result of the calculation. Therefore, rather than having to do the same work again using the same input, it can simply return the value stored in the cache.

**⚠️ Common Mistakes**

* Without memoization, we cannot create a cache where we store inputs with their calculated results. Whenever we have an input that we’ve already seen, we can simply retrieve the result rather than redoing any of our work. By creating a cache, we’re using additional space, so you have to decide whether that’s worth the improved speed. If you have a very large range of inputs where it’s fairly unlikely that you’ll need to repeat the same calculations, memoization may not be an efficient solution.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.


```markdown
1) if the number is even divide it by 2 and continue
2) if the number is odd take the two possible numbers and divide it by 2 and use the one that results in an even number since odd adds an extra step.
3) When the resultant num when divided by 2 in step 2 is 1 use the smallest num.
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution(object):
    def integerReplacement(self, n):
        if n <= 1:
            return 0
        level = 0
        while n != 1:
            if n & 1 == 0:
                n, level = n / 2, level + 1
            elif n == 3:
                n, level = n-1, level + 1
            else:
                if (n-1)/2 & 1 == 0:
                    n, level = n-1, level + 1
                else:
                    n, level = n+1, level + 1                    
        return level   
```

``java
public int integerReplacement(int n) {
    if (n == Integer.MAX_VALUE) return 32; //n = 2^31-1;
    int count = 0;
    while (n > 1){
        if (n % 2 == 0) n  /= 2;
        else{
            if ( (n + 1) % 4 == 0 && (n - 1 != 2) ) n++;
            else n--;
        }
        count++;
    }
    return count;
}   
```


    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(n)
* **Space Complexity**: O(n)