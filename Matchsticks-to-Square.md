## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/matchsticks-to-square/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: ___ mins
* 🛠️ **Topics**: Backtracking
* 🗒️ **Similar Questions**: [Maximum Rows Covered by Columns](https://leetcode.com/problems/maximum-rows-covered-by-columns/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- 

Run through a set of example cases:

```markdown
Example 1
Input: matchsticks = [1,1,2,2,2]
Output: true
Explanation: You can form a square with length 2, one side of the square came two sticks with length 1.

Example 2:
Input: matchsticks = [3,3,3,3,4]
Output: false
Explanation: You cannot find a way to form a square with all the matchsticks.
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.



## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** 

```markdown
1) Calculate sum of the array. If sum/4 is odd, there can not be four subsets with equal sum, so return false.

2) If sum of array elements is even, calculate sum/4 and check if there are subsets of array with sum equal to sum/4 in a recursive manner.
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?


## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def makesquare(self, nums):
    """
    :type nums: List[int]
    :rtype: bool
    """
    target, rem = divmod(sum(nums), 4)
    if rem:
        return False
    
    def getSides(i, target):
        # return all the possible ways we can make a side
        if target == 0:
            return [[]]
        if i == len(nums) or target < 0:
            return []
        else:
            include = [[i] + c for c in getSides(i + 1, target - nums[i])]
            return include + getSides(i + 1, target)
        
    groups = getSides(0, target)
    if len(groups) < 4:
        return False
    else:
        usage = {i:0 for i in range(len(nums))}
        for g in groups:
            for i in g:
                usage[i] += 1
        for index in usage:
            if usage[index] == 0 or usage[index] + 3 > len(groups):
                return False
    return True
```

```java
public class Solution {
    public boolean makesquare(int[] nums) {
        
        // Calculate sum of the elements in array
        if(nums.length==0)
            return false;
        
        
        int sum = 0;
        int n=nums.length;
        for (int i = 0; i < n; i++)
            sum += nums[i];
 
        // If sum is odd, there cannot be two subsets
        // with equal sum
        if (sum%4 != 0)
            return false;
        
        
        
        return isSubsetSum (nums, n, sum/4, sum/4, sum/4, sum/4);
    
    }
    
    public static boolean isSubsetSum (int arr[], int n, int sum1, int sum2, int sum3, int sum4){
        // Base Cases
        if (sum1<0 || sum2<0 || sum3<0 || sum4<0 )
        	return false;
        if (n==0 && (sum1 == 0 && sum2 == 0 && sum3==0 && sum4==0))
            return true;
        if (n == 0 && (sum1 != 0 || sum2 != 0 || sum3 != 0 || sum4 != 0 ))
            return false;
 
        
        if (arr[n-1] > sum1 && arr[n-1]>sum2 && arr[n-1]>sum3 && arr[n-1]>sum4 )
        	return false;
 
        /* else, check if sum can be obtained by any of
           the following
        (a) including the last element
        (b) excluding the last element
        */
        return isSubsetSum (arr, n-1, sum1-arr[n-1], sum2, sum3, sum4) ||
                isSubsetSum (arr, n-1, sum1, sum2-arr[n-1], sum3, sum4) ||
                isSubsetSum (arr, n-1, sum1, sum2, sum3 - arr[n-1], sum4) ||
                isSubsetSum (arr, n-1, sum1, sum2, sum3, sum4 - arr[n-1]);
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.


The complexity analysis based on how many elements there are in generateParenthesis(n).

* **Time Complexity**: O(2^N) where N is the length of M for the attempted combinations of elements in M
* **Space Complexity**: O(N) for the recursion stack
