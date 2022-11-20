## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/rabbits-in-forest/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Greedy
* 🗒️ **Similar Questions**: [TBD
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can I see what color the rabbits are?
  - No
- What are the constraints?
  - 1 <= answers.length <= 1000
  - 0 <= answers[i] < 1000
- If there are fewer than i + 1 answers of i, they could be the same color
  - There could be a group of 2 rabbits who say "1"
  - There could be a group of 3 rabbits who say "2"
- If a rabbit says:
  - 0: add 1 to our count every time
  - 1: add 2 to our count every two rabbits
  - 2: add 3 to our count every 3 rabbits
  - n: add n+1 to our count every n rabbits
   
```markdown
HAPPY CASE
Input: answers = [1,1,2]
Output: 5

EDGE CASE
Input: answers = [10,10,10]
Output: 11
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For graph problems, we want to consider the following approaches:

* Greedy Algorithm: Greedily put as many rabbits into the same color as possible, until it is not possible to fit, create a new color. If rabbits share same answer, we can put at most rabbits in this color group. Because the result is min of rabbits count, so put as many rabbits in same color group as possible.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** 

```markdown
1. Create a hashtable counts
2. Set sum = 0
3. For each answer in answers
If answer == 0, sum += 1
If answer not in counts:
counts[answer] = 1
sum += answer + 1
If counts[answer]  < answer + 1
counts[answer] ++
else
counts[answer] += 1
sum += answer + 1
4. Return sum
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

To minimize the total number of rabbits, we can divide these y rabbits into groups, each group having a size of (x+1) and the rabbits within the same group having the same color. If y%(x+1)>0, then the last y%(x+1) rabbits have the same color as some other rabbits that don't share their information in this poll. These last y%(x+1) and those rabbits that don't participate should add up to (x+1) (Otherwise, the claim that x other rabbits have the same color is a false claim).

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution(object):
    def numRabbits(self, answers):
        count = collections.Counter(answers)
        return sum(-v % (k+1) + v for k, v in count.iteritems())
```
```java
class Solution {
    public int numRabbits(int[] answers) {
        int[] count = new int[1000];
        for (int x: answers) count[x]++;

        int ans = 0;
        for (int k = 0; k < 1000; ++k)
            ans += Math.floorMod(-count[k], k+1) + count[k];
        return ans;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

* **Time Complexity**: O(N), where N is the number of rabbits that answered
* **Space Complexity**: O(N)