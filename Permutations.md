## Problem Highlights

* 🔗 **Leetcode Link:** [Permutations](https://leetcode.com/problems/permutations/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Array, Recursion, Backtracking
* 🗒️ **Similar Questions**: [Next Permutation](https://leetcode.com/problems/next-permutation/), [Permutations II](https://leetcode.com/problems/permutations-ii/), [Permutation Sequence](https://leetcode.com/problems/permutation-sequence/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:
* What is a permutation?
    * A permutation is nothing but an arrangement of given integers.
* What permutations are we returning in this problem?
    * We return all possible arrangements of the given sequence.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

Input: nums = [0,1]
Output: [[0,1],[1,0]]

EDGE CASE 
Input: nums = [1]
Output: [[1]]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Sort
- Two pointer solutions (left and right pointer variables)
- Storing the elements of the array in a HashMap or a Set
- Traversing the array with a sliding window
- Loop through the array, in each iteration, a new number is added to different locations of results of previous iteration. Start from an empty List.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** The concept is to resolve the problem recursively by thinking of finding permutations as mainly swapping values in a list. Look at it as set theory that there are N! permutations of a list of size N. We also know that the permutations are going to be the leaves of the tree, which means we will have N! leaves. In order to get to each one of those leaves, we had to go through N calls. That's O(N*N!). Again a little more than the total number of nodes because some nodes are shared among more than one path.


```markdown
1. Pick one element in the list, and remove it from the list of available integers
2. Generate all the permutations for the remaining list and append them to the first element
3. To do that we keep track of current which is the currently available integers, and permutation, which is the currently generated permutation.
```

⚠️ **Common Mistakes**

* Given an array, the challenge is to find all of the unique combinations of the values in that array and return that information in a new array. Therefore, the result should be an array of arrays. That also means that the length of the results array should also equal the factorial of the length of the given array. For example, if [1, 2, 3] was given, then I should have an array of 6 arrays (3 x 2 x 1).
The best way to think about the problem is to imagine each index being moved into each position of the array until all of the combinations have been satisfied at least once.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        n = len(nums)
        
        def solve(n, perm):
            if n==0:
                res.append(perm.copy())
                return None
            for i in range(len(nums)):
                # To do that we keep track of current which is the currently available integers, and permutation, which is the currently generated permutation.
                if nums[i] in perm: continue

                # Generate all the permutations for the remaining list and append them to the first element
                perm.append(nums[i])

                solve(n-1, perm)
                # Pick one element in the list, and remove it from the list of available integers
                perm.pop()
                
        solve(n, [])
        return res
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(N*N!) 
```markdown
1 x 2 + 2 x 3 + 6 x 4 + ... + (n-1)! x n = 1! x 2 + 2! x 3 + 3! x 4 + ... + (n-1)! x n = 2! + 3! + 4! + ... + n!
```
* **Space Complexity**: O(n!) because we're storing n! permutations
