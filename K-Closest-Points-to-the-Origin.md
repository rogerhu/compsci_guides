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
def k_closest_points (points, K):
    heap = []
    for x,y in points:
        d = -(x*x + y*y)
        heapq.heappush(heap, (d,x,y))
        if len(heap) > K:
            heapq.heappop(heap)

    ret = []
    for d,x,y in heap:
        ret.append((x,y))

    return ret
```
```java
class Solution {
    public int[][] kClosest(int[][] points, int k) {
        PriorityQueue<Integer> pq = new PriorityQueue<>(
            new Comparator<Integer>(){
                @Override
                public int compare(Integer a, Integer b){
                return a-b;
                }
            }
        );

        Map<Integer, int[]> map = new HashMap<>();
        for(int[] point : points){
            int d = getDistance(point);
            map.put(d, point);
            pq.offer(d);
        }

        int[][] res = new int[k][2];
         for(int i=0; i<k && !pq.isEmpty(); i++){
            int d = pq.poll();
            int[] arr = map.get(d);
            res[i] = arr;
         }      
        return res;
    }

    private int getDistance(int[] points){
        return (int)(Math.pow(Math.abs(points[0]),2) + Math.pow(Math.abs(points[1]),2));
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
