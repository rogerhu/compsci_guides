## Problem Highlights

* 🔗 **Leetcode Link:** [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array, Heap 
* 🗒️ **Similar Questions**: [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/), [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/),[Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Will k be smaller or equal to nums?
    - Yes, k is always smaller or equal to nums
- What is the space and time complexity?
    - We want O(n) time and O(n) space. 

```markdown
HAPPY CASE
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5

Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Output: 4

EDGE CASE
Input: nums = [3], k = 1
Output: 3
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - The runtime will already be nlogn with the first sort, this is over our runtime expectations.
- Two pointer solutions (left and right pointer variables). 
    - We appoarch this array from both sides, but that requires sorting.
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap or Set just complicates our code.
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.
- Heap
    - Lets use a heap to and remove k times, this removes k largest elements and puts the kth largest element at the top of the heap. The cost is O(k) time which is constant and less than or equal to O(n) time. That will meet our runtime expectations.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We will heapify the nums, so that we can identify the largest stones at all times. We will remove k items to get the kth largest item at the top of the heap

```markdown
1. Heapify the num array, create new array with negative values for each num, because python only supports minimum heaps.
2. Remove k items from heap
3. Return the top of the heap. 
```

**⚠️ Common Mistakes**

* We want to ask for space/time complexity. Yes this is an easy problem if we had O(nlogn) time. But the interviewer wants to solve this problem in O(k) time.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        # Heapify the num array, create new array with negative values for each num, because python only supports minimum heaps.
        heap = [-num for num in nums]
        heapq.heapify(heap)

        # Remove k items from heap
        for _ in range(1,k):
            heapq.heappop(heap)
        
        # Return the top of the heap.
        return neg(heap[0])
```
```java
class Solution {
    public int findKthLargest(int[] nums, int k) 
    {
        // Heapify the num array, create new array with negative values for each num
        PriorityQueue<Integer> minheap=new PriorityQueue<>();
        for (int i = 0; i < k; i++)
            minheap.add(nums[i]);
        
        // Remove len(nums) - k items from heap
        for (int i = k; i < nums.length; i++)
        {
            if (nums[i]>minheap.peek())
            {
                minheap.poll();
                minheap.add(nums[i]);
            }
        }

        // Return the top of the heap.
        return minheap.peek();
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

* **Time Complexity**: `O(N)` because we generating a heap requires `O(N)` time. The removal of K items need on `O(K)` time, `O(K)` may be less than or equal to `O(N)`.
* **Space Complexity**: `O(N)` because we need to generate and store a heap. 