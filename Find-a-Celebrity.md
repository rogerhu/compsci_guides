## Problem Highlights

* 🔗 **Leetcode Link:** [Find the Celebrity](https://leetcode.com/problems/find-the-celebrity/) 
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Two Pointers, Greedy, Graphs
* 🗒️ **Similar Questions**: [Find the Town Judge](https://leetcode.com/problems/find-the-town-judge/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could there be 0 people at the party? What happens if there is just 1 person and they know themself?
  - In the 0 people case, there is no celebrity. We should return -1. On the other hand, the 1 person case that you have listed, technically, has a celebrity.
- Could there be 2 celebrities present at the party?
  - There cannot be 2 celebrities. Since a celebrity, by definition, is known by everyone, the other celebrity would have to know them. This means one of the two potential celebrities isn’t in fact a celebrity.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: graph = [[1,1,0],[0,1,0],[1,1,1]]
Output: 1

Input: graph = [[1,0,1],[1,1,0],[0,1,1]]
Output: -1

EDGE CASE
Input: 0 (No people at the party)
Output: -1
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Stack: What could we push onto a stack to make this problem easier? Stacks don’t allow us to keep track of data based on keys.
- Queue: Queues fall into the same category as Stacks, do we need to maintain any sense of ordering to solve this problem?
- HashMap: HashMaps allow us to store data for quick access. What could we store in a HashMap to make this problem easier?
- Heap: Do we need some sort of ordering to our data that a Heap could provide?
- Grid: This problem doesn’t specify a search that would be easier to solve with a grid-like structure.
- Adjacency Matrix: We would have to populate an Adjacency Matrix by using the knows(a, b) function. However, is it necessary to call this function O(N^2) times? Could we find the celebrity in less time? In less than O(N^2) space as well?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use a stack to iterate through all the individuals at the party. Pop the stack and call knows(a, b) on individuals till a celebrity is found.

```markdown
1) Create a stack and push elements 0 --> N-1 onto the stack
2) Create two variables left and right
3) Pop the first two elements into left and right respectively
4) While the stack has individuals on it:
    a) If left knows right, left cannot be a celebrity
        i) Pop the next element into left
        ii) Refer to right as a potential celebrity
    b) If left does not know right, right cannot be a celebrity
        i) Pop the next element into right
        ii) Refer to left as a potential celebrity
5) The potential celebrity reference is the most valid individual
4) Loop through all individuals and verify:
    a) Potential celebrity does not know anyone
    b) Everyone except the potential celebrity knows the potential celebrity
5) If valid, return celebrity, else return -1
```

⚠️ **Common Mistakes**

* Some people tend to understand this may be a graph problem. This leads them to believe they need to reconstruct the graph entirely, which leads to excess calls to knows(a, b). If this occurs, encourage student to see if there is a way to solve this problem without re-constructing a graph.
* Some people tend to see the underlying graph structure to this problem immediately. Try to avoid tunnel vision by showing them different ways the data could be re-arranged. Such as a list of numbers from 0 --> N-1.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def findCelebrity(n: int) -> int:
        # base case
	if n < 2:
		return -1
	stack = []
        # put all people to the stack
	for i in range(n):
		stack.append(i)
	left = stack.pop()
	right = stack.pop()
	potential_celebrity = left if not knows(left, right) else right
	while len(stack) > 0:
		if knows(left, right):
			potential_celebrity = right
			left = stack.pop()
		else:
			potential_celebrity = left
			right = stack.pop()
        # double check the potential celebrity
	for i in range(n):
		if i != potential_celebrity and (not knows(i, potential_celebrity) or knows(potential_celebrity, i)):
			return -1
	return potential_celebrity
```
```java
public class Solution extends Relation {
    public int findCelebrity(int n) {
    // base case
    if (n <= 0) return -1;
    if (n == 1) return 0;
    
    Stack<Integer> stack = new Stack<>();
    
    // put all people to the stack
    for (int i = 0; i < n; i++) stack.push(i);
    
    int a = 0, b = 0;
    
    while (stack.size() > 1) {
        a = stack.pop(); b = stack.pop();
        
        if (knows(a, b)) 
            // a knows b, so a is not the celebrity, but b may be
            stack.push(b);
        else 
            // a doesn't know b, so b is not the celebrity, but a may be
            stack.push(a);
    }
    
    // double check the potential celebrity
    int c = stack.pop();
    
    for (int i = 0; i < n; i++)
        // c should not know anyone else
        if (i != c && (knows(c, i) || !knows(i, c)))
            return -1;
    
    return c;
 }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(n), where n is the number of API calls. For each of the n people, we need to check whether or not they are a celebrity. Checking whether or not somebody is a celebrity requires making 2 API calls for each of the n - 1 other people.

* **Space Complexity**: O(n), for the stack used. 