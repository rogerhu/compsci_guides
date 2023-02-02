## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/maximum-subarray/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Dynamic Programming
* 🗒️ **Similar Questions**: [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/), [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- How can we sum negative numbers?
  - We can have a variable to store sum of the positive subarray which ends at current iterated element, and another variable to store maximum sum of the positive contiguous subarray till current iterated element.
- How do you get the location of the maximum subarray?
  - You can maintain variables of max_start and max_end with the help of variables current_start and current_end
   
```markdown
HAPPY CASE
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6

Input: nums = [5,4,-1,7,8]
Output: 23

EDGE CASE
Input: nums = [1]
Output: 1
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

Whenever you see a question that asks for the maximum or minimum of something, consider Dynamic Programming as a possibility. If you want to forego the brute force approach, a more optimal solution for obtaining the maximum sub-array is [Kadane’s algorithm](https://medium.com/@rsinghal757/kadanes-algorithm-dynamic-programming-how-and-why-does-it-work-3fd8849ed73d). Two significant variables used is: current_maximum to keep track of whether or not the value at the current index would increase the maximum sum and maximum_so_far to keep track of the overall maximum that is propagated along the array.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

```markdown
1. Initialize 2 integer variables. Set both of them equal to the first value in the array.
currentSubarray will keep the running count of the current subarray we are focusing on.
maxSubarray will be our final return value. Continuously update it whenever we find a bigger subarray.
2. Iterate through the array, starting with the 2nd element (as we used the first element to initialize our variables). For each number, add it to the currentSubarray we are building. If currentSubarray becomes negative, we know it isn't worth keeping, so throw it away. Remember to update maxSubarray every time we find a new maximum.
3. Return maxSubarray.
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

The algorithm will find the continuous subarray in the 1D integer array which has the largest sum possible. The first approach for everyone after understanding the problem statement will be applying the brute-force approach and solving the problem. But by doing so, the time complexity of the solution will be O(N^2) which isn't ideal. We need to solve the problem by traversing over the whole array using two variables to track the sum so far and maximum total.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # initialize our variables using the first element.
        current_subarray = max_subarray = nums[0]
        
        # start with the 2nd element since we already used the first one.
        for num in nums[1:]:
            # if current_subarray is negative, throw it away. Otherwise, keep adding to it.
            current_subarray = max(num, current_subarray + num)
            max_subarray = max(max_subarray, current_subarray)
        
        return max_subarray
```
```java
class Solution {
    public int maxSubArray(int[] nums) {
        // initialize our variables using the first element.
        int currentSubarray = nums[0];
        int maxSubarray = nums[0];
        
        // start with the 2nd element since we already used the first one.
        for (int i = 1; i < nums.length; i++) {
            int num = nums[i];
            // if current_subarray is negative, throw it away. Otherwise, keep adding to it.
            currentSubarray = Math.max(num, currentSubarray + num);
            maxSubarray = Math.max(maxSubarray, currentSubarray);
        }
        
        return maxSubarray;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(N), where N is the length of nums.
* **Space Complexity**: O(1), No matter how long the input is, we are only ever using 2 variables: currentSubarray and maxSubarray.