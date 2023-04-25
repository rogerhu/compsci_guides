## Problem Highlights

* 🔗 **Leetcode Link:** [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Recursion, DP, Bottom-Up
* 🗒️ **Similar Questions**: [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/), [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/), [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- How many days can buy and sell stocks?
    - 999 days
- What are the minimum number days?
    - 1 day, there is no need to account for edges case of zero days.
- What is the time and space complexity for this problem?
    - `O(n)` time and `O(1)` space will do. 


Run through a set of example cases:

```markdown
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

Input: prices = [7,6,4,3,1]
Output: 0
Explanation: In this case, no transactions are done and the max profit = 0.

EDGE CASE 
Input: cost = [1]
Output: 0
```   
    
## 2: M-atch

> **Match**  what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- 2D Array Top-Left -> Bottom-Right, Bottom-Up DP Technique
    - We are not working with a 2D Array, but we can use the Bottom-Up DP Technique to build up our answers from previous calculation.
- Knapsack-Type Approach
    - This does not fit the knapsack-type approach.
- Cache previous results, generally
    - Yes, we can use the Top-Down DP Technique and recursion to build our answers from previous recursive calls, but that would be a round about way of solving this algorithm when the array of prices was already given.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Bottom-Up DP Technique, we will build up our answer. We will find the minimum cost at each date and check for maximum profit sold.

```markdown

1. Create variables to hold minimum prices and maximum profit
2. As we progress through the array of prices
    a. Record the minimum price and build minimum prices array as we proceed through the array
    b. Check for a higher profit
3. Return the maximum profit.
```

**General Idea:** Bottom-Up DP Technique, we will build up our answer, and reduce variables. Again, we will find the minimum cost at each date and check for maximum profit sold.

```markdown
1. Create variables to hold minimum price and maximum profit
2. As we progress through the array of prices
    a. Record the minimum price found as we proceed through the array
    b. Check for a higher profit
3. Return the maximum profit.
```

⚠️ **Common Mistakes**

* This problem may looks quite simple at the first glance, however we are using the fundamentals of dynamic programming to record past results to be reused in future calculation. We were able to bring down the cost from O(n^2) to O(n), by avoiding a seek for lowest possible buy price for each sell price. 

## 4: I-mplement

> **Implement** the code to solve the algorithm.

**Solution 1**

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # Create variables to hold minimum prices and maximum profit
        maxProfit = 0
        memoMinPrice = [prices[0]]

        # As we progress through the array of prices
        for i in range(len(prices)):
            # Record the minimum price and build minimum price as we proceed through the array
            memoMinPrice.append(min(prices[i], memoMinPrice[i]))
            # Check for a higher profit
            maxProfit = max(maxProfit, prices[i] - memoMinPrice[i])
        
        # Return the maximum profit.
        return maxProfit
```
```java
class Solution {
  public int maxProfit(int[] prices) {
    // Create variables to hold minimum prices and maximum profit
    int[] memoMinPrice = new int[prices.length + 1];
    memoMinPrice[0] = prices[0];
    int maxProfit = 0;

    // As we progress through the array of prices
    for (int i = 0; i < prices.length; i++) {
      // Record the minimum price found as we proceed through the array
      memoMinPrice[i + 1] = (Math.min(memoMinPrice[i], prices[i]));
      // Check for a higher profit
      maxProfit = Math.max(maxProfit, prices[i] - memoMinPrice[i]);
    }
    
    // Return the maximum profit.
    return maxProfit;
  }
}
```

**Solution 2**

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        # Create variables to hold minimum prices and maximum profit
        maxProfit = 0
        minPrice = prices[0]

        # As we progress through the array of prices
        for price in prices:
            # Record the minimum price found as we proceed through the array
            minPrice = min(price, minPrice)
            # Check for a higher profit
            maxProfit = max(maxProfit, price - minPrice)
        
        # Return the maximum profit.
        return maxProfit
```
```java
class Solution {
  public int maxProfit(int[] prices) {
    // Create variables to hold minimum prices and maximum profit
    int minValue = prices[0];
    int maxProfit = 0;

    // As we progress through the array of prices
    for (int i = 0; i < prices.length; i++) {
      // Record the minimum price found as we proceed through the array
      minValue = Math.min(minValue, prices[i]);
      // Check for a higher profit
      maxProfit = Math.max(maxProfit, prices[i] - minValue);
    }
    
    // Return the maximum profit.
    return maxProfit;
  }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of item in the array

**Solution 1**

* **Time Complexity**: `O(N)`, because we need to build up to check each item in the array.
* **Space Complexity**: `O(N)`, because we held array of information regarding minPrice at each index.

**Solution 2**

* **Time Complexity**: `O(N)`, because we need to build up to check each item in the array.
* **Space Complexity**: `O(1)`, because we held 2 pieces of information minPrice and maxProfit.
