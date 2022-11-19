## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/trapping-rain-water/](https://leetcode.com/problems/trapping-rain-water/)
* 💡 **Problem Difficulty:** Hard
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Array
* 🗒️ **Similar Questions**: [Container With Most Water](https://leetcode.com/problems/container-with-most-water/), [Trapping Rain Water II](https://leetcode.com/problems/trapping-rain-water-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- Could no water be trapped in any of the cells?
  - That is definitely possible.
- Can water be held on the edges of the array?
  - No. Let’s assume the edges can’t really hold water because there is nothing outside the array holding water in those positions. Does that make sense?
- Could an index have a negative value?
  - Let’s assume all the values are non-negative.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: [1, 3, 4, 5, 0] 
Output: 0

Input: [1, 0, 2, 4, 2, 4] 
Output: 3

EDGE CASE
Input: [0, 0, 0] 
Output: 0
```   
    
## 2: M-atch

> **Match** 

For this step, we expect the interviewees will think or verbalize about common techniques or related problems to the given problem.

In Strings/Arrays, common problem patterns include:

- Hash and Store: What would we hash at each index? If we do hash, how does that help us compute the amount of water we can trap.
- Two Pointer: Two pointer may help us reference both sides of the array at the same time. Since we know that water can be trapped at a cell that has elements on both sides that are higher than itself. This could help us in this situation.
- Sort: Since the order of the elements matters, sorting may not result in a potential solution path.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We must maintain `left_max` and `right_max` during the iteration, using 2 pointers, switching between the two.

```markdown
1) Create Left and Right pointers, water variable
2) Keep track of the maxes from both Left and Right sides
3) Move the smaller pointer towards the center
    a) With the smaller pointer, update the correct side maximum value
    b) Calculate water at the smaller pointer spot, update return variable
        i) Calculate water storage with the smaller maximum side value (L/R)
4) Return total water amount
```

⚠️ **Common Mistakes**

* Some people may try to rush this problem by knowing array techniques they may already know. However, understanding the logic behind conditions that allow water to be trapped is the most important aspect to this problem.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution(object):
    def trap(self, height):
	# two pointers at beginning and end of array
        left = 0
        right = len(height) - 1
		
        higher_level = 0 # keep track of "highest" level of water seen so far
        water = 0 # total water
		
        while left < right:
            if height[left] <= height[right]:
		# if left pointer has lower level, store the value and move it one step to right
                lower_level = height[left]
                left += 1
            else:
		# if right pointer has lower level, store the value and move it one step to left
                lower_level = height[right]
                right -= 1
				
	    # make sure to store and continuously update the current "highest" level of water
            higher_level = max(higher_level, lower_level)
			
            # incrementally keep track of amount of water so far
            water += higher_level - lower_level
			
        return water
```
```java
class Solution {
    public int trap(int[] height) {
        int res=0;
        int p1=0;
        int p2=height.length-1;
        int leftMax=height[p1];
        int rightMax=height[p2];
        // edge case when array has 0 or 1 element in it
        if(height.length <=1) {
            return res;
        }
        while(p1<=p2) {
            if(leftMax<=rightMax) {
                // if leftMax - current element is positive then add it to res
                int temp = Math.max(leftMax-height[p1],0);
                res+=temp;
                // update leftmax
                leftMax=Math.max(leftMax,height[p1]);
                p1++;
            } else {
                // if rightMax - current element is positive then add it to res
                int temp = Math.max(rightMax-height[p2],0);
                res+=temp;
                // update rightMax
                rightMax=Math.max(rightMax,height[p2]);
                p2--;
            }
        }
        return res;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(n), where n is 
* **Space Complexity**: O(1), only constant space required for left, right, left_max, and right_max.
