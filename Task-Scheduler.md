## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/task-scheduler/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Greedy
* 🗒️ **Similar Questions**: [Rearrange String k Distance Apart](https://leetcode.com/problems/rearrange-string-k-distance-apart/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can we solve this without sorting?
  - Notice that the absence of sorting does not affect the running time analysis of the algorithm as the cost of sorting an array of fixed size is constant.
- Can we use a hashmap here?
  - The main idea is to record frequencies in a hashmap, rather than an array. Then, we loop through the key-value pairs. In the iteration, for all keys other than the key with the maximum frequency, we try to decrease the idle time.
- What if the idle task drops below 0?
  - If the idle item drops below 0 at any time, we know that tasks.length is the answer as there can't be idle time lower than 0.
   
```markdown
HAPPY CASE
Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation: 
A -> B -> idle -> A -> B -> idle -> A -> B
There is at least 2 units of time between any two same tasks.

Input: tasks = ["A","A","A","B","B","B"], n = 0
Output: 6
Explanation: On this case any permutation of size 6 would work since n = 0.
["A","A","A","B","B","B"]
["A","B","A","B","A","B"]
["B","B","B","A","A","A"]
...
And so on.

EDGE CASE
Input: tasks = ["A","A","A","A","A","A","B","C","D","E","F","G"], n = 2
Output: 16
Explanation: 
One possible solution is
A -> B -> C -> A -> D -> E -> A -> F -> G -> A -> idle -> idle -> A -> idle -> idle -> A
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

* Sort
* Two pointer solutions (left and right pointer variables)
* Storing the elements of the array in a HashMap or a Set
* Traversing the array with a sliding window
* We can schedule most frequent tasks without idling using greedy. The total number of CPU intervals we need consists of busy and idle slots. Number of busy slots is defined by the number of tasks to execute: len(tasks). The problem is to compute a number of idle slots.

Maximum possible number of idle slots is defined by the frequency of the most frequent task: `idle_time <= (f_max - 1) * n`.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** 

```markdown
1. The maximum number of tasks is 26. Let's allocate an array frequencies of 26 elements to keep the frequency of each task.
2. Iterate over the input array and store the frequency of task A at index 0, the frequency of task B at index 1, etc.
3. Sort the array and retrieve the maximum frequency f_max. This frequency defines the max possible idle time: idle_time = (f_max - 1) * n.
4. Pick the elements in the descending order one by one. At each step, decrease the idle time by min(f_max - 1, f) where f is a current frequency. Remember, that idle_time is greater or equal to 0.
5. Return busy slots + idle slots: len(tasks) + idle_time.
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

- To work on the same task again, CPU has to wait for time n, therefore we can think of as if there is a cycle, of time n+1, regardless whether you schedule some other task in the cycle or not. To avoid leave the CPU with limited choice of tasks and having to sit there cooling down frequently at the end, it is critical the keep the diversity of the task pool for as long as possible. In order to do that, we should try to schedule the CPU to always try round robin between the most popular tasks at any time.


## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        # frequencies of the tasks
        frequencies = [0] * 26
        for t in tasks:
            frequencies[ord(t) - ord('A')] += 1
        
        frequencies.sort()

        # max frequency
        f_max = frequencies.pop()
        idle_time = (f_max - 1) * n
        
        while frequencies and idle_time > 0:
            idle_time -= min(f_max - 1, frequencies.pop())
        idle_time = max(0, idle_time)

        return idle_time + len(tasks)

```
```java
class Solution {
    public int leastInterval(char[] tasks, int n) {
        // frequencies of the tasks
        int[] frequencies = new int[26];
        for (int t : tasks) {
            frequencies[t - 'A']++;
        }

        Arrays.sort(frequencies);

        // max frequency
        int f_max = frequencies[25];
        int idle_time = (f_max - 1) * n;
        
        for (int i = frequencies.length - 2; i >= 0 && idle_time > 0; --i) {
            idle_time -= Math.min(f_max - 1, frequencies[i]); 
        }
        idle_time = Math.max(0, idle_time);

        return idle_time + tasks.length;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume N represents the number of items in array,

* **Time Complexity**: O(N), this time is needed to iterate over the input array tasks and compute the array frequencies. Array frequencies contains 26 elements, and hence all operations with it takes constant time.
* **Space Complexity**: O(1), to keep the array frequencies of 26 elements.