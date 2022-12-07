## Problem Highlights

* 🔗 **Leetcode Link:** [Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/) 
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array, Queue
* 🗒️ **Similar Questions**: [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/), [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/), [Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can we expect negative numbers in the pings?
  - No, the pings will alway be positive numbers.

- Can we expect numbers to be in increasing order?
  - It is guaranteed that every ping uses a strictly larger value of t than the previous ping.

- Can we expect an empty array as input? What should we return in this case?
  - No, we will assume that there is at least one ping.
   
```markdown
HAPPY CASE
Input
["RecentCounter", "ping", "ping", "ping", "ping"]
[[], [1], [100], [3001], [3002]]
Output
[null, 1, 2, 3, 3]

Explanation
RecentCounter recentCounter = new RecentCounter();
recentCounter.ping(1);     // requests = [1], range is [-2999,1], return 1
recentCounter.ping(100);   // requests = [1, 100], range is [-2900,100], return 2
recentCounter.ping(3001);  // requests = [1, 100, 3001], range is [1,3001], return 3
recentCounter.ping(3002);  // requests = [1, 100, 3001, 3002], range is [2,3002], return 3

Input
["RecentCounter", "ping", "ping", "ping", "ping"]
[[], [1], [100], [300], [400],[4000]]
Output
[null, 1, 2, 3, 4, 1]

EDGE CASE
Input
["RecentCounter", "ping", "ping", "ping", "ping"]
[[], [1]]
Output
[null, 1]
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

If you are dealing with Arrays, there are some common techniques you can employ to help you solve the problem:
- Sort
    - Will sorting the array help you solve the problem?
- Two pointer solutions (left and right pointer variables)
    - Will two pointer help you record pings.

- Storing the elements of the array in a HashMap or a Set
    - Will hashmap or hashset give you the ordering required for storing pings within 3000 milliseconds.

- Traversing the array with a sliding window
    - Will a restrictive window help us?

- Use a Queue
    - A queue will allow us to add a new ping and remove any ping in our queue that has more than 3000 milliseconds compared to our new ping.
## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use a queue to record pings. Remove any ping that is more than 3000 milliseconds of the new ping

```markdown
1. Create a new queue
2. Upon Ping add new ping to queue
3. Remove all ping in queue with value more than 3000 away from new ping
4. Return length of queue
```

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class RecentCounter:

    def __init__(self):
        # Create a new queue
        self.queue = deque()

    def ping(self, t: int) -> int:
        # Upon Ping add new ping to queue
        self.queue.append(t)

        # Remove all ping in queue with value more than 3000 away from new ping
        while self.queue and self.queue[0] + 3000 < t:
            self.queue.popleft()
        
        # Return length of queue
        return len(self.queue)
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of ping in queue 
    
* **Time Complexity**: O(N), we may need to remove N pings from the queue. 
    * Side Node, we can improve this time complexity to O(logN) with binary search and pop off a chuck of the queue at a time.
* **Space Complexity**: O(N), we need to store N pings in our queue.