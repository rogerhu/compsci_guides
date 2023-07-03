## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/basic-calculator/](https://leetcode.com/problems/basic-calculator/)
* 💡 **Problem Difficulty:** Medium
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: String
* 🗒️ **Similar Questions**: [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/), [Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

Be sure that you clarify the input and output parameters of the problem:

- Does space affect the evaluation of an input expression?
  - Spaces do not affect the evaluation of the input expression
- Does the input contain valid strings?
  - Input always contains valid strings


Run through a set of example cases:

```markdown
HAPPY CASE
Example 1:
Input: s = "1 + 1"
Output: 2

Example 2:
Input: s = " 2-1 + 2 "
Output: 3

EDGE CASE
Example 3:
Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23
```   
    
## 2: M-atch

> **Match** 

For this string problem, we can think about the following techniques:

- Sort If the given string is given in a proper order, the string can be are sorted in a specified arrangement.

- Two pointer solutions (left and right pointer variables) A two pointer solution would be used if you are searching pairs in a sorted array.

- Storing the elements of the array in a HashMap or a Set A hashmap will allow us to store object and retrieve it in constant time, provided we know the key.

- Traversing the array with a sliding window Using a sliding window is iterable and ordered and is normally used for a longest, shortest or optimal sequence that satisfies a given condition.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Whenever we see a (, put the intermediate result into the stack, and start new calculation right after this (. When we see a ), pop the add/minus the result with the last item in the stack.

```markdown
1. Iterate the expression string in reverse order one character at a time. 

2. Once we encounter a character which is not a digit, we push the operand onto the stack. When we encounter an opening parenthesis (, this means an expression just ended. 

3. Push the other non-digits onto to the stack.

4. Do this until we get the final result.
```

⚠️ **Common Mistakes**

* A tricky aspect of this problem includes handling a unary operator, such as "-3+3", "4+(-5)". For example, consider 12-3+(4-(5+6)) and what happens to array:

We start with [0], the current sum of the top level being zero.
The two ) push one zero each so we're at [0, 0, 0].
The 6 gets pushed: [0, 0, 0, 6]
The + adds the 6 to the parent level: [0, 0, 6]
The 5 gets pushed: [0, 0, 6, 5]
The ( adds the 5 to the parent level: [0, 0, 11]. Note that this is the same as if the input were 12-3+(4-11) and we had just processed the 11. So we already fully processed that innermost parenthesis expression (5+6) correctly.
The - subtracts the 11 from the parent level: [0, -11].
The 4 gets pushed: [0, -11, 4]
The ( adds the 4 to the parent level: [0, -7].
The + adds the -7 to parent level: [-7].
The 3 gets pushed: [-7, 3]
The - subtracts the 3 from the parent level: [-10]
The 12 gets pushed: [-10, 12]
Now we have processed the whole input string and we have running sum -10 at the base level and the 12 hasn't been incorporated yet. Just add them to get the final result -10+12=2.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def calculate(self, s: str) -> int:
        nums_stack = []
        ops_stack =[]
        
        cur_num = "0"
        cur_op = "+"
        
        running_result = 0
        for i in range(len(s)):
            
            c = s[i]
            
            if c.isdigit():
                cur_num += c
            
            elif c in ["+", "-"]:
                # before we replace cur_op, calculate previous num and put it into result 
                running_result  += -1*int(cur_num) if cur_op == "-" else int(cur_num)
                
                #refresh both
                cur_num= "0" 
                cur_op = c

            elif c == "(":
                nums_stack.append(running_result)
                ops_stack.append(cur_op)
                
                running_result= 0
                cur_op ="+"
                cur_num = "0"
                
                
            elif c == ")":
                running_result += -1*int(cur_num) if cur_op == "-" else int(cur_num)
                
                # take out previous context
                prev_num = nums_stack.pop() if nums_stack else  "0"
                prev_op = ops_stack.pop() if ops_stack else "+"
                
                
                # update the new result
                if prev_op =="-":
                    running_result = -1*running_result
                    
                running_result = prev_num + running_result
                
                #refresh the context
                cur_num= "0"
                cur_op ="+"
                
        #left over
        if cur_num: running_result += -1*(int(cur_num)) if cur_op =="-" else int(cur_num)
            
        return running_result
```
```java
public class Solution {
    public int calculate(String s) {
        Stack<Integer> numsStack = new Stack<>();
        Stack<Character> opsStack = new Stack<>();
        
        String curNum = "0";
        char curOp = '+';
        
        int runningResult = 0;
        
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            
            if (Character.isDigit(c)) {
                curNum += c;
            }
            else if (c == '+' || c == '-') {
                runningResult += (curOp == '-') ? -Integer.parseInt(curNum) : Integer.parseInt(curNum);
                
                curNum = "0";
                curOp = c;
            }
            else if (c == '(') {
                numsStack.push(runningResult);
                opsStack.push(curOp);
                
                runningResult = 0;
                curOp = '+';
                curNum = "0";
            }
            else if (c == ')') {
                runningResult += (curOp == '-') ? -Integer.parseInt(curNum) : Integer.parseInt(curNum);
                
                int prevNum = (!numsStack.isEmpty()) ? numsStack.pop() : 0;
                char prevOp = (!opsStack.isEmpty()) ? opsStack.pop() : '+';
                
                if (prevOp == '-') {
                    runningResult = -runningResult;
                }
                
                runningResult = prevNum + runningResult;
                
                curNum = "0";
                curOp = '+';
            }
        }
        
        if (!curNum.equals("")) {
            runningResult += (curOp == '-') ? -Integer.parseInt(curNum) : Integer.parseInt(curNum);
        }
        
        return runningResult;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.
   

* **Time Complexity**: O(N), where N is the length of the string
* **Space Complexity**: O(N) to account for stack used
