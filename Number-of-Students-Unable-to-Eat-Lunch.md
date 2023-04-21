## Problem Highlights

* 🔗 **Leetcode Link:** [Number of Students Unable to Eat Lunch](https://leetcode.com/problems/number-of-students-unable-to-eat-lunch/)
* 💡 **Difficulty:** Easy
* ⏰ **Time to complete**: 25 mins
* 🛠️ **Topics**: Array, Queue, Stack 
* 🗒️ **Similar Questions**: [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/), [Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/), [Time Needed to Buy Tickets](https://leetcode.com/problems/time-needed-to-buy-tickets/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Will there always be an equal amount of sandwiches to students?
    - Yes, sandwiches are always equal to students.
- Students form a queue and sandwiches form a stack?
    - Yes, that is correct. You can only take sandwiches and students from the 0th position. And return students to the nth position.
- What is the space and time complexity?
    - We want `O(N)` time and `O(1)` space. Where `N` is the number of students. 

```markdown
HAPPY CASE
Input: students = [1,1,0,0], sandwiches = [0,1,0,1]
Output: 0 
Explanation:
- Front student leaves the top sandwich and returns to the end of the line making students = [1,0,0,1].
- Front student leaves the top sandwich and returns to the end of the line making students = [0,0,1,1].
- Front student takes the top sandwich and leaves the line making students = [0,1,1] and sandwiches = [1,0,1].
- Front student leaves the top sandwich and returns to the end of the line making students = [1,1,0].
- Front student takes the top sandwich and leaves the line making students = [1,0] and sandwiches = [0,1].
- Front student leaves the top sandwich and returns to the end of the line making students = [0,1].
- Front student takes the top sandwich and leaves the line making students = [1] and sandwiches = [1].
- Front student takes the top sandwich and leaves the line making students = [] and sandwiches = [].
Hence all students are able to eat.

Input: students = [1], sandwiches = [0]
Output: 1

EDGE CASE
Input: students = [1,1,1,0,0,1], sandwiches = [1,0,0,0,1,1]
Output: 3
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Sort. 
    - The runtime will already be nlogn with the first sort, this is over our runtime expectations.
- Two pointer solutions (left and right pointer variables). 
    - We appoarch this array from both sides, but we can't because stacks and queues.
- Storing the elements of the array in a HashMap or a Set. 
    - A HashMap or Set just complicates our code.
- Traversing the array with a sliding window. Similar to the two pointer solution. 
    - A sliding window doesn't really help us here.
- Queue
    - Use the idea of a queue and process students and sandwiches. 

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** We will use the idea of queue and stack to solve this problem. Our limiting data structure is the stack. So we need the queue of students to accommadate the stack. As long as have a count of student with round sandwich and square sandwich we can remove them from queue. It doesn't matter where they are in the queue. 


```markdown
1. Create a count of students with round sandwich and square sandwich.
2. Start at the top of stack of sandwiches and distribute sandwich to appropriate student
3. Once no more students want the top sandwich then we cannot distribute sandwiches and we are left with students without lunch.
4. Return the total number of students without lunch. 
```

**⚠️ Common Mistakes**

* We want to ask for space/time complexity. Yes this is an easy problem if we had O(n^2) time. But the interviewer wants to solve this problem in O(n) time and O(1) space.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

Basic Solution:

```python
class Solution:
    def countStudents(self, students: List[int], sandwiches: List[int]) -> int:
        # Create a count of students with round sandwich and square sandwich.
        squareStudents = roundStudents = 0
        for student in students:
            if student == 0:
                squareStudents += 1
            else:
                roundStudents += 1
        
        # Start at the top of stack of sandwiches and distribute sandwich to appropriate student
        # Once no more students want the top sandwich then we cannot distribute sandwiches and we are left with students without lunch.
        for sandwich in sandwiches:
            if sandwich == 0:
                if squareStudents == 0:
                    break
                else:
                    squareStudents -= 1
            elif sandwich == 1:
                if roundStudents == 0:
                    break
                else:
                    roundStudents -= 1
        
        # Return the total number of students without lunch. 
        return roundStudents + squareStudents
```
```java
class Solution {
    public int countStudents(int[] students, int[] sandwiches) {
        // Create a count of students with round sandwich and square sandwich.
        int ones = 0; //count of students who prefer type1
        int zeros = 0; //count of students who prefer type0
		
        for(int student : students){
            if(student == 0) zeros++;
            else ones++;
        }
        
        // Start at the top of stack of sandwiches and distribute sandwich to appropriate student
        // Once no more students want the top sandwich then we cannot distribute sandwiches and we are left with students without lunch
        for(int sandwich : sandwiches){
            if(sandwich == 0){  // if sandwich is of type0
                if(zeros == 0){ // if no student want a type0 sandwich
                    break;
                }
                zeros--;
            }
            else{  // if sandwich is of type1
                if(ones == 0){  // if no student want a type1 sandwich 
                    break;
                }
                ones--;
            }
        }
        // Return the total number of students without lunch.
        return zeros + ones;
    }
}
```

Advanced Python Solution:

```python
class Solution:
    def countStudents(self, students: List[int], sandwiches: List[int]) -> int:
        # Create a count of students with round sandwich and square sandwich.
        counter = Counter(students)
        
        # Start at the top of stack of sandwiches and distribute sandwich to appropriate student
        # Once no more students want the top sandwich then we cannot distribute sandwiches and we are left with students without lunch.
        for sandwich in sandwiches:
            if counter[sandwich] == 0:
                break
            counter[sandwich] -= 1
        
        # Return the total number of students without lunch. 
        return counter[0] + counter[1]
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of items in the sandwich/student array.

* **Time Complexity**: `O(N)` because we need to go through every sandwich.
* **Space Complexity**: `O(1)` because we only need to count the number of students. 