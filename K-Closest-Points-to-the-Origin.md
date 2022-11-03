## Problem Highlights

* 🔗 **Leetcode Link:** [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Array, Sorting, Heap
* 🗒️ **Similar Questions**: [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/), [Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What if one of the points is the same as the origin?
    - The distance would be zero, and that would be one of the closest ones presumably.
- What if there's duplicate distances, say K is one and there's two that are the same, which do you want me to return? Or do you want me to return both?
    - Doesn't matter. You return up to k.
- How would you modify the solution if the input was an infinite streak of points?
- What are the Time/Space Constraints Considerations?
    - Try to seek different uses of O(N) space to solve this problem!

How would you modify the solution if the input was an infinite streak of points?
Time/Space Constraints Considerations:
Try to seek different uses of O(N) space to solve this problem!


Run through a set of example cases:

```markdown
HAPPY CASE
Input: points = [[1,3],[-2,2]], k = 1
Output: [[-2,2]]

Input: points = [[3,3],[5,-1],[-2,4]], k = 2
Output: [[3,3],[-2,4]]

EDGE CASE
Input: [[1,1], [1,1], [1,1], [0,0]], k = 2
Output: [[0,0], [1,1]]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Stack: What could we push onto a stack to make this problem easier? Stacks don’t allow us to keep track of data based on keys.
- Queue: Queues fall into the same category as Stacks, do we need to maintain any sense of ordering to solve this problem?
- HashMap: HashMaps allow us to store data for quick access. What could we store in a HashMap to make this problem easier?
- Heap: Do we need some sort of ordering to our data that a Heap could provide?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** According to the distance of the point from the origin, use a priority queue to store the coordinates of the point in a priority queue of pairs. To assign the maximum priority to the least distant point from the origin, we use the Comparator class in Priority Queue. Next, we can print the first K elements of the priority queue.

```markdown
1. Place the points into a PriorityQueue 
2. Whenever the size reach K + 1, poll the farthest point out
3. Then for each point, we add it to the heap.
4. If the heap top is far from the origin compared to the incoming point then add the incoming point in the heap and remove the earlier top.
```

⚠️ **Common Mistakes**

* Some people may try to approach this problem initially with some brute force O(N^2) strategy. However, this approach doesn’t work. Try to urge students to avoid usual brute force tactics. Instead, urge them to use known data structures to solve this problem.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
import heapq

class Solution:
    def kClosest(self, points: List[List[int]], k: int) -> List[List[int]]:
        heap = []  # out heap
        output = [] # output array since we can't return heap directly + don't want to modify the points array
        
        # loop through the points array
        for cord in points:
            # calculate the distance from 0 - current point
            distance = ((cord[0] - 0) ** 2) + ((cord[1] - 0) ** 2)
            # make a tuple, where tuple[0] is the distance negated, and tuple[1] is the current point we're on eg. (-8, [-2, 2])
            # note that heapq will keep the heap property of tuples using the first index.
            '''
            we negate the distance because python's built in heap is by default a min heap. 
            we also want to be able to utilize heapq.heappushpop, and heapq.heappush, which doesn't exist for
            the builtin max heap implementation.
            '''
            distance_tuple = (-distance, cord)
            
            if len(heap) == k:
                # check if we've reached our heap's max size, which is k.
                # if so, we push the current tuple and pop the smallest item. 
                # since values are negated, the smallest item ends up being the largest item
                heapq.heappushpop(heap, distance_tuple)
            else:
                # simply push on to the heap if we haven't exceeded size k
                heapq.heappush(heap, distance_tuple)
        
        # add points arrays from the tuples in the heaps to our output to get desired answer
        for item in heap:
            output.append(item[1])

        return output
```
```java
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        
        PriorityQueue<int[]> maxHeap = new PriorityQueue<>((a, b) -> (b[0] * b[0] + b[1] * b[1]) - (a[0] * a[0] + a[1] * a[1]));
        
        // loop in the rows of the 2Darray called points
        for(int[] point : points) {
            
            // add a row to the max heap
            maxHeap.add(point);
            
            // if the size of the max heap becomes more than k then remove a row from it
            if(maxHeap.size() > k) 
                maxHeap.remove();
        }
        
        int[][] res = new int[k][2];
        
        while(k-- > 0) {
            
            // the last element in the heap is the kth closest element to the origin so put it in the kth row of res
            res[k] = maxHeap.remove();
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
    
* **Time Complexity**: O(1) for all stack operations
* **Space Complexity**: O(N) total stack space used
