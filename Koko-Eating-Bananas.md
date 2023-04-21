## Problem Highlights

* 🔗 **Leetcode Link:** [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Binary Search 
* 🗒️ **Similar Questions**: [Binary Search](https://leetcode.com/problems/binary-search/), [First Bad Version](https://leetcode.com/problems/first-bad-version/), [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Will there be fewer piles than hours given to eat bananas?
    - Yes, piles are equal or less than hours given to eat bananas. There is no need to check for edge case where it is impossible to eat all piles with hours given.

- What is the space and time complexity?
    - We want O(m * logn) time and O(1) space. You will have to figure out what m and n means.


```markdown
HAPPY CASE
Input: piles = [3,6,7,11], h = 8
Output: 4

Input: piles = [30,11,23,4,20], h = 5
Output: 30

EDGE CASE
Input: piles = [23,23,4,20], h = 4
Output: 23
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - The range of possible numbers is already sorted, we can use binary search on this. Just think of the minimum number as 1 and maximum number as the largest number in the pile.
- Two pointer solutions (left and right pointer variables). 
    - We can have a left and right pointer to create a mid point where we can decide whether or not the number is in the upper half or lower half of the sorted range. 
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap or Set just complicates our code.
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We can have a left and right pointer to create a mid point where we can distinguish between the smaller and larger half, where we can find the minimum number of bananas needs to eat per hour and finish all the piles of bananas 

```markdown
1. Get the minimum number of bananas needed per hour / lower bound of binary search
2. Get the maximum number of bananas needed per hour / higher bound of binary search
3. While left pointer is less than or equal to right pointer we have not exhausted the possible numbers
    a. Get the mid point of the two pointers and test if this number of bananas per hour will finish all piles within time limit
    b. Check if the number of banana is enough to eat per hour and finish all the piles of banana.
        i. If we can finish the piles of banana, then lets try to reduce the number of banana per hour, move right pointer to mid point. The answer must be midpoint and above.
        ii. If we cannot finish the piles of banana, then lets try to increase the number of bananas per hour, move left pointer to mid point. The answer cannot be left of midpoint.
4. Return the minimum number of bananas per hour.
```

**⚠️ Common Mistakes**

* We want to ask for space/time complexity. Yes this is an easy problem if we had O(n^2) time. But the interviewer wants to solve this problem in O(n * logn) time.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        # Get the minimum number of bananas needed per hour / lower bound of binary search
        left = ceil(sum(piles) / h) 

        # Get the maximum number of bananas needed per hour / higher bound of binary search
        right = max(piles) 

        # While left pointer is less than or equal to right pointer we have not exhausted the possible numbers
        while left < right:
            # Get the mid point of the two pointers and test if this number of bananas per hour will finish all piles within time limit
            mid = (left + right) // 2 

            # Check if the number of banana is enough to eat per hour and finish all the piles of banana.
            total_time = 0
            for i in piles:
                total_time += ceil(i / mid)
                if total_time > h:
                    break

            # If we can finish the piles of banana, then lets try to reduce the number of banana per hour, move right pointer to mid point. The answer must be midpoint and above.
            if total_time <= h:
                right = mid 
            # If we cannot finish the piles of banana, then lets try to increase the number of bananas per hour, move left pointer to mid point. The answer cannot be left of midpoint.
            else:
                left = mid + 1

        # Return the minimum number of bananas per hour.
        return right
```
```java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        // Get lower bound of binary search
        int left = 1;
        // Get higher bound of binary search
        int right = 1000000000;
        
        // While left pointer is less than or equal to right pointer we have not exhausted the possible numbers
        while(left < right){
            // Get the mid point of the two pointers and test if this number of bananas per hour will finish all piles within time limit
            int mid = left + (right - left) / 2;
            
            // Check if the number of banana is enough to eat per hour and finish all the piles of banana.

            // If we can finish the piles of banana, then lets try to reduce the number of banana per hour, move right pointer to mid point. The answer must be midpoint and above.
            if(canEatInTime(piles, mid, h)) right = mid;

            // If we cannot finish the piles of banana, then lets try to increase the number of bananas per hour, move left pointer to mid point. The answer cannot be left of midpoint.
            else left = mid + 1;
        }

        // Return the minimum number of bananas per hour.
        return right;
    }
    public boolean canEatInTime(int piles[], int k, int h){
        int hours = 0;
        for(int pile : piles){
            int div = pile / k;
            hours += div;
            if(pile % k != 0) hours++;
        }
        return hours <= h;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of possible bananas eaten per hour. `M` represents the number of piles of bananas

* **Time Complexity**: `O(M * logN)` because we can eliminate half the number of possible bananas eaten per hour with each check. And each check require looking every pile. 
* **Space Complexity**: `O(1)` because we only need a few variables to do the job.