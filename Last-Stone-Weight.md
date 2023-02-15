## Problem Highlights

* 🔗 **Leetcode Link:** [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array, Heap 
* 🗒️ **Similar Questions**: [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/), [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/), [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Will there be at least one stone?
    - Yes, the number stones will always be one or more

- What is the space and time complexity?
    - We want O(nlogk) time and O(n) space. 


```markdown
HAPPY CASE
Input: stones = [2,7,4,1,8,1]
Output: 1
Explanation: 
We combine 7 and 8 to get 1 so the array converts to [2,4,1,1,1] then,
we combine 2 and 4 to get 2 so the array converts to [2,1,1,1] then,
we combine 2 and 1 to get 1 so the array converts to [1,1,1] then,
we combine 1 and 1 to get 0 so the array converts to [1] then that's the value of the last stone.

Input: stones = [2,7,4,9,8,1,1]
Output: 0
Explanation: 
We combine 9 and 8 to get 1 so the array converts to [2,7,4,1,1,1] then,
we combine 7 and 4 to get 3 so the array converts to [2,3,1,1,1] then,
we combine 2 and 3 to get 1 so the array converts to [1,1,1,1] then,
we combine 1 and 1 to get 0 so the array converts to [1,1] then,
we combine 1 and 1 to get 0 so the array converts to [] then that's the value of the last stone.

EDGE CASE
Input: stones = [1]
Output: 1
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - The runtime will already be nlogn with the first sort, this is over our runtime expectations.
- Two pointer solutions (left and right pointer variables). 
    - We appoarch this arrya from both sides, but that requires sorting.
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap or Set just complicates our code.
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.
- Heap
    - Lets use a heap to get the largest two items each time. The cost is logk. That will meet our runtime expectations.

**⚠️ Common Mistakes**

* We want to ask for space/time complexity. Yes this is an easy problem if we had O(n*nlogn) time. But the interviewer wants to solve this problem in O(nlogk) time.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We will heapify our array of stones, so that we can identify the two largest stones at all times. We will smash the two largest stones until one or less stones exist.


```markdown
1. Heapify the stones array
    a. Create new array with negative values for each stone, because python only supports minimum heaps.
2. While there is more than one item in heap. 
    a. Obtain two largest stones
    b. Smash the two stones
    c. If result is 0, then continue
    d. If result is more than 0, then push result back into heap
3. If there is still one stone, then return it. Otherwise return 0.
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:
        # Heapify the stones array
        # Create new array with negative values for each stone, because python only supports minimum heaps.
        heap = [-stone for stone in stones]
        heapq.heapify(heap)

        while len(heap) > 1:
            # Obtain two largest stones
            stone1 = heapq.heappop(heap)
            stone2 = heapq.heappop(heap)

            # Smash the two stones
            result = abs(stone2 - stone1)

            # If result is 0, then continue
            if result == 0:
                continue
            # If result is more than 0, then push result back into heap
            else:
                heapq.heappush(heap, -result)

        # If there is still one stone, then return it. Otherwise return 0.
        return neg(heap[0]) if len(heap) > 0 else 0
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the array.

* **Time Complexity**: `O(nlogk)` because we need to access each stone and each access requires logk, so `O(nlogk)`
* **Space Complexity**: `O(n)` because we need to produce a heap. 