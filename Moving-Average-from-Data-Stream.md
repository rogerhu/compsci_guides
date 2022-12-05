## Problem Highlights

* 🔗 **Leetcode Link:**  [Moving Average from Data Streamstack](https://leetcode.com/problems/moving-average-from-data-stream/)
* **Difficulty:** Easy
* **Time to complete**: 20 mins
* **Topics**: Array, Stack
* **Similar Questions**: [K Radius Subarray Averages](https://leetcode.com/problems/k-radius-subarray-averages/), [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Do we need to keep all values form the data stream?
    - No. Notice that we do not need to keep all values from the data stream, but rather the last n values which falls into the moving window.
- What is a new number is added to the window?
    - If the new number comes in, the size is already full, which is equal to the window size. We would then need to remove the first number, and next, add the new number. Think FIFO.
- How can I maintain the window and calculate the sum?
    - To calculate the sum, we do not need to reiterate the elements in the moving window. We could keep the sum of the previous moving window, then in order to obtain the sum of the new moving window, we simply add the new element and deduce the oldest element.
   
```markdown
HAPPY CASE
Input
["MovingAverage", "next", "next", "next", "next"]
[[3], [1], [10], [3], [5]]
Output
[null, 1.0, 5.5, 4.66667, 6.0]

Input
["MovingAverage", "next", "next", "next", "next"]
[[4], [1], [10], [3], [5]]
Output
[null, 1.0, 5.5, 4.66667, 4.75]
 
 
EDGE CASE
Input
["MovingAverage", "next", "next", "next", "next", "next"]
[[0], [1], [0], [1], [0], [1]]
Output
[null, 1.0, 0.5, 0.667, 0.5, 0.6]
*Assuming a rolling sum initialized with zero should average the whole thing*
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

This problem does not follow a strict, standard pattern. However, urge students to consider the fundamental differences between Stacks and Queues:

- Order of Data
    - This is a critical difference that can be used to solve this problem
- Runtimes of Access/Placement operations

Following this intuition, by definition of the moving window, at each step, we add a new element to the window, and at the same time we remove the oldest element from the window. We replace the queue with the deque and add a window sum variable in order to calculate the sum of moving window in constant time.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Keep a running total and remove items first in first out using a queue



```markdown
1) Create a variable for sum
2) Create a queue
3) Set a size
4) Add item to queue 
5) Remove an item from queue if queue's size getting too big. We need to remove the first coming number from the window and calculate the avg, so a queue can be used here to ensure the first in first out
6) add the number that come in and calculate the avg
```

**⚠️ Common Mistakes**

* Look out to pop and push element to the right Stack
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
 
from collections import deque
 
class MovingAverage:

    def __init__(self, size):
        """
        Initialize your data structure here.
        """ 
        # Create a variable for sum
        self.total = 0
        
        # Create a queue
        self.queue = deque()
        
        # Set a size
        self.size = size

    def next(self, val):
        # Add item to queue 
        self.queue.append(val)
        
        # Remove an item from queue if queue's size getting too big. We need to remove the first coming number from the window and calculate the avg, so a queue can be used here to ensure the first in first out
        if len(self.queue) > self.size:
            self.total -= self.queue.popleft()
        
        # add the number that come in and calculate the avg
        self.total += val
        return (self.total / len(self.queue))
```
```java
public class movingAverage {
    private Queue<Integer> queue;
    private int maxSize;
    private double sum;
    /** Initialize your data structure here. */
    public MovingAverage(int size) {
        // Create a variable for sum
        sum = 0.0;

        // Create a queue
        queue = new LinkedList<Integer>();

        // Set a size
        maxSize = size;
    }

    public double next(int val) {
        //  Remove an item from queue if queue's size getting too big. We need to remove the first coming number from the window and calculate the avg, so a queue can be used here to ensure the first in first out
        if(queue.size() == maxSize)
            sum -= queue.remove();

        // Add item to queue 
        queue.add(val);

        // add the number that come in and calculate the avg
        sum += val;
        return sum / queue.size();
    }
}
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of elements in the running stream and `Q` represents the size of the queue

* **Time Complexity**: `O(N)` because we will need to access each element in the running stream.
* **Space Complexity**: `O(Q)` because we may need to store all items up until the size of the queue.