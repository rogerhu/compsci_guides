For some programming problems you may find that there is a 'greedy' way to solve it. A greedy approach means that at every step we take the best option for us with the current circumstances.

For example, consider the following situation:
**There is a pile of coins containing: 2 quarters, 3 dimes, and 10 pennies**
We are allowed to take 3 coins from the pile. **How do we maximize our profit**? The greedy approach would be to **always take the coin with the highest value**. 
In this case, the **first coin we take would be a quarter**. We would **then take another quarter**. Our **third coin would be a dime** since that is now the highest value coin.

This greedy approach can also be applied to a handful of common problems. When appropriate, the greedy approach is a great way to solve a problem. However, the difficulty lies in recognizing whether a problem can be correctly solved greedily.

### Advantages:
- Simplicity: Greedy algorithms are often easier to visualize and code. As a result, they can also be easier to analyze in terms of space and time complexity.
- Efficiency: Greedy algorithms usually don't involve having to store a lot of data or going through multiple passes of the input

### Disadvantages:
- Hard to design: The hard part often lies in trying picture the problem in a way that would allow you to solve it greedily.
- Proving correctness: Proving that the greedy approach will be correct in all cases can also be a challenge

### Common problems that can be solved with the greedy approach
#### Frog Jumping
```
Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

Example:
Input: [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

https://leetcode.com/articles/jump-game/#approach-4-greedy-accepted

#### Activity Selecting

```
You are given n activities with their start and finish times. 
Select the maximum number of activities that can be performed by a single person, 
assuming that a person can only work on a single activity at a time.
```

https://www.geeksforgeeks.org/greedy-algorithms-set-1-activity-selection-problem/

#### Job Sequencing / Task Scheduler
```
Given a char array representing tasks CPU need to do. 
It contains capital letters A to Z where different letters represent different tasks.
Tasks could be done without original order. 
Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.

However, there is a non-negative cooling interval n that means between two same tasks, 
there must be at least n intervals that CPU are doing different tasks or just be idle.

You need to return the least number of intervals the CPU will take to finish all the given tasks.

Example 1:
Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
Explanation: A -> B -> idle -> A -> B -> idle -> A -> B.
```

https://leetcode.com/problems/task-scheduler/description/

#### Huffman Coding
```
Huffman coding is a lossless data compression algorithm. 
The idea is to assign variable-length codes to input characters, 
lengths of the assigned codes are based on the frequencies of corresponding characters. 

The most frequent character gets the smallest code and the least frequent character gets the largest code.
```
https://www.geeksforgeeks.org/greedy-algorithms-set-3-huffman-coding/
### Resources:
https://web.stanford.edu/class/archive/cs/cs161/cs161.1138/lectures/13/Small13.pdf

https://www.topcoder.com/community/data-science/data-science-tutorials/greedy-is-good/