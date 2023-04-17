## Problem Highlights

* 🔗 **Leetcode Link:** [First Bad Version](https://leetcode.com/problems/first-bad-version/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Sorted Array, Binary Search 
* 🗒️ **Similar Questions**: [Binary Search](https://leetcode.com/problems/binary-search/), [Sqrt(x)](https://leetcode.com/problems/sqrtx/), [Search in a Sorted Array of Unknown Size](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input array be empty?
    - No, there will alway be at least one number.
- Will there alway be one bad version?
    - Yes, there will always be one bad version
- What is the space and time complexity?
    - We want O(logn) time and O(1) space. 


```markdown
HAPPY CASE
Input: n = 5, bad = 4
Output: 4
Explanation:
call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true
Then 4 is the first bad version.

Input: n = 5, bad = 5
Output: 5
Explanation:
call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> false
call isBadVersion(5) -> true
Then 5 is the first bad version.


EDGE CASE
Input: n = 1, bad = 1
Output: 1
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - The arrays are already sorted.
- Two pointer solutions (left and right pointer variables). 
    - We can have a left and right pointer to create a mid point where we can decide whether or not the version is bad in the left half or right half. Binary Search
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap or Set just complicates our code.
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.

**⚠️ Common Mistakes**

* We want to ask for space/time complexity. Yes this is an easy problem if we had O(n) time. But the interviewer wants to solve this problem in O(logn) time.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We can have a left and right pointer to create a mid point where we can decide whether or not the number exist in the left half or right half, with each iteration until we find our number or we exhaust our list. Binary Search


```markdown
1. Initialize left and right pointers
2. While left pointer is less than right pointer we have not exhausted the versions
    a. Get the mid point of the two pointers 
    b. Check if mid point is bad or not
        i. If mid point is bad then there might be more bad versions before it, shift right pointer and check for earlier bad version.
        ii. If mid point is good then we know that everything to the left is good, shift left pointer to check for bad version in right half
3. Return the right pointer for first bad version. 
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
# The isBadVersion API is already defined for you.
# def isBadVersion(version: int) -> bool:

class Solution:
    def firstBadVersion(self, n: int) -> int:
        # Initialize left and right pointers
        l, r = 1, n
        # While left pointer is less than right pointer we have not exhausted the versions
        while l < r:
            # Get the mid point of the two pointers
            mid = (l + r) // 2

            # Check if mid point is bad or not
            if isBadVersion(mid):
                # If mid point is bad then there might be more bad versions before it, shift right pointer and check for earlier bad version.
                r = mid
            else:
                # If mid point is good then we know that everything to the left is good, shift left pointer to check for bad version in right half
                l = mid + 1
        # Return the right pointer for first bad version
        return r
```
```java
 public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        // Initialize left and right pointers
        int l=1;
        int r=n;
        // While left pointer is less than right pointer we have not exhausted the versions
        while(l<r){
            // Get the mid point of the two pointers
            int m=l+(r-l)/2;

            // Check if mid point is bad or not
            if(isBadVersion(m)){
                // If mid point is bad then there might be more bad versions before it, shift right pointer and check for earlier bad version.
                r=m;
            }else{
                // If mid point is good then we know that everything to the left is good, shift left pointer to check for bad version in right half
                l=m+1;
            }
        }
        // Return the right pointer for first bad version
        return r;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array.

* **Time Complexity**: `O(logN)` because we can eliminate half the possible versions with each check.
* **Space Complexity**: `O(1)` because we only need two pointers to do the job.