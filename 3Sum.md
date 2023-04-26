## Problem Highlights

* 🔗 **Leetcode Link:** [3Sum](https://leetcode.com/problems/3sum/)
* 💡 **Difficulty:**  Medium
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array, Two Pointer
* 🗒️ **Similar Questions**: [Number of Arithmetic Triplets](https://leetcode.com/problems/number-of-arithmetic-triplets/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Is this question similar to [Two Sum](https://leetcode.com/articles/two-sum/) and [Two Sum II](https://leetcode.com/articles/two-sum-ii-input-array-is-sorted/)?
  - Yes, it's a good idea to take a first look at [Two Sum](https://leetcode.com/articles/two-sum/) and [Two Sum II](https://leetcode.com/articles/two-sum-ii-input-array-is-sorted/). Two Sum, Two Sum II and 3Sum share a similarity that the sum of elements must match the target exactly. A difference is that, instead of exactly one answer, we need to find all unique triplets that sum to zero.
- Can you modify the input array?
  - Yes, you can especially if you want to avoid copying it due to memory constraints.


```markdown
HAPPY CASE
Example 1:
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]

Example 2:
Input: nums = [0,1,1]
Output: []

EDGE CASE
Example 3:
Input: nums = [0,0,0]
Output: [[0,0,0]]
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - Sort the array so we can skip repeated values.
- Two pointer solutions (left and right pointer variables). 
    - It requires the array to be first sorted. To make sure the result contains unique triplets, we need to skip duplicate values. It is easy to do because repeating values are next to each other in a sorted array.
 ), sorting the array would not change the overall time complexity. Repeat until we reach index 0 for nums2.
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap or Set just complicates our code.
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.

**⚠️ Common Mistakes**

* The requirements asks to return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`. You may notice that there a solution where `j == k` is possible. `i`, `j` and `k` are indexes and not the elements of the array. You cannot use same element more than once in the triplet, but you can definitely use same number on different indexes. Remember you've to return triplet of elements and not triplet of indexes unlike the two sum problem where you've to return pair of indexes.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

```markdown
First, sort the array.
Have two pointers l and r where l starts from i + 1 and r in reverse. ie len(nums)-1. Here i is the index of n.
As the array is sorted, if n + nums[l] + nums[r] > 0 then decreare r or else decrease l.
If equal to zero, append to ans.


n == nums[i-1] explanation:
if 2 numbers are same, skip the number.
eg [-1,0,1,2,-1,-4]
After sorting, [-4,-1,-1,0,1,2]
Here -1 is repeated. So for i = 2, n = -1, nums[i-1] == n. Thus move to next step (or else it will create duplicates)

```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        ans = []
        nums.sort()

        for i,n in enumerate(nums):
            if i > 0 and n == nums[i-1]:
                continue

            l, r = i + 1, len(nums) - 1
            while l < r:
                threeSum = n + nums[l] + nums[r]
                if threeSum > 0:
                    r -= 1
                elif threeSum < 0:
                    l += 1
                else:
                    ans.append([n,nums[l],nums[r]])
                    l += 1
                    r -= 1
                    while nums[l] == nums[l - 1] and l < r:
                        l += 1

        return ans
```

```java
class Solution {
    public List<List<Integer>> threeSum(int[] arr) {
        
        Arrays.sort(arr);
        
        List<List<Integer>> ls = new ArrayList<>();
        
        for (int i = 0; i<arr.length-2; i++) {
            if (i == 0 || (i>0 && arr[i-1]!=arr[i])) { //check for duplicates to avoid copy we've used arr[i-1]!=arr[i] instead of arr[i+1]!=arr[i] because we must take the duplicate value in condition i.e. in example 1 [-1,-1,2] is also and output so if we do arr[i+1]!=arr[i] then we'll skip to the next -1 and this output will not come
            int start = i+1;
            int end = arr.length-1;
			int sum = 0-arr[i];
            
            while (start<end) {
                if (arr[start] + arr[end] == sum) {
                    ls.add(Arrays.asList(arr[i], arr[start], arr[end]));
                    
                    while (start<end && arr[start] == arr[start+1]) start++;//avoid all the same values
                    while (start<end && arr[end] == arr[end-1]) end--;//avoid all the same values
                    
                    start++;
                    end--;
                } else if (arr[start] + arr[end] < sum) {
                    start++;
                } else end--;
            }
            }
        }
        
        return ls;
    }
    
    
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Given n elements, 

* **Time Complexity**: O(n^2). Sorting the array takes O(nlog⁡n), so overall complexity is O(nlog⁡n+n2)+ n^2.
* **Space Complexity**: O(n)