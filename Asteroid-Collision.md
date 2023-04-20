## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/asteroid-collision/](https://leetcode.com/problems/asteroid-collision/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Array 
* 🗒️ **Similar Questions**: [Can Place Flowers](https://leetcode.com/problems/can-place-flowers/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- How is this a LIFO scenario?
  - Since the collisions happen for the last in asteroid that will need to be first out if it is destroyed, a stack (LIFO) can be used.
- What is an example of a negative asteroid?
  - Negative asteroids without any positive asteroids on the left can be ignored as they will never interact with the upcoming asteroids regardless of their direction.
- What is an example of a positive asteroid? 
  - Positive asteroids (right-moving) may interact with negative asteroids (left-moving) that come later.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: asteroids = [5,10,-5]
Output: [5,10]

Input: asteroids = [8,-8]
Output: []
Explanation: The 8 and -8 collide exploding each other.

EDGE CASE
Input: asteroids = [10,2,-5]
Output: [10]
Explanation: The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.
```   
    
## 2: M-atch

> **Match** 

For this string problem, we can think about the following techniques:

- Sort If the given string is given in a proper order, the string can be are sorted in a specified arrangement.

- Two pointer solutions (left and right pointer variables) A two pointer solution would be used if you are searching pairs in a sorted array.

- Storing the elements of the array in a HashMap or a Set A hashmap will allow us to store object and retrieve it in constant time, provided we know the key.

- Traversing the array with a sliding window
Using a sliding window is iterable and ordered and is normally used for a longest, shortest or optimal sequence that satisfies a given condition.

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We can use a stack to efficiently simulate the collisions.

```markdown
1. Declare a stack
2. Pick an asteroid one by one from the list of asteroids
3. Pay attention to an asteroid if it is negative, else simply push it into the stack. Analyze the Stack from top to bottom and do some operations on it until our desired condition is met.
Those operations are:
a) If the top of the Stack has a lesser magnitude than the current asteroid, we destroy (pop) it.
b) Else If the top of the stack is Negative or the Stack got empty, push the current asteroid.
c) Else If the top of the stack has the same magnitude and has a positive sign, pop it, and also destroy the current asteroid.
d) Else If the top of the stack is positive and has a greater magnitude, destroy the current asteroid (i.e., do no changes to the stack and pick the next asteroid)
```

⚠️ **Common Mistakes**

* Now for the catch - a (-) asteroid will keep crushing smaller positive asteroids, so there's 4 cases to handle:

1. keep crushing until no more smaller (+)
2. push to stack if empty or meets a (-)
3. destroy both if they're of equal mass
4. destroy incoming (-) if it meets a larger (+) (note: this is handled by doing nothing after the first three)

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def asteroidCollision(self, asteroids: List[int]) -> List[int]:
        stack = []
        for asteroid in asteroids:
            # if there's things on the stack, we need to consider if we've got case 4
            while stack and stack[-1] > 0 and asteroid < 0:
		# determine which asteroids are exploding
                if abs(stack[-1]) < abs(asteroid):
                    stack.pop()
		    # considered asteroid might still destroy others so continue checking
                    continue
                elif abs(stack[-1]) == abs(asteroid):
                    stack.pop()
                break
	    # if nothing on the stack, or cases 1-3, just append
            else:
                stack.append(asteroid)
        return stack
```
```java
class Solution:
    def asteroidCollision(self, asteroids: List[int]) -> List[int]:
        stack = []
        for ast in asteroids:
            if stack and ast < 0 < stack[-1]:
                # collision occurs if +ast top of stack meets -ast coming in
                while stack and stack[-1] > 0 and stack[-1] < abs(ast):
                    # keep crushing smaller +ast
                    stack.pop()
                if not stack or stack[-1] < 0:
                    # no more to crush or meets -ast
                    stack.append(ast)
                elif stack[-1] == abs(ast):
                    # equal explosion
                    stack.pop()           
            else:
                stack.append(ast)
        
        return stack

```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
    
* **Time Complexity**: O(n), where n is the number of asteroids
* **Space Complexity**: O(n), since we used a stack to keep track of the intermediate results. 