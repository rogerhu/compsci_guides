## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/generate-parentheses/>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Backtracking
* 🗒️ **Similar Questions**: [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- If solved recursively, what is the time complexity?
  - The time complexity is O(2^n). For every index there can be two options { or }. So it can be said that the upper bound of time complexity is O(2^n) or it has exponential time complexity
0 How do we apply the divide and conquer principle to this problem?
  - The combinations can be obtained by dividing the string into two parts, the left part would be embraced by a pair of parenthesis, and then appended with the right right without parenthesis, we get a combination.

Run through a set of example cases:

```markdown
HAPPY CASE
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]

Input: n = 1
Output: ["()"]

EDGE CASE 
Input: n = 0
Output:  [""]
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

* Can we generate only permutations which we know for sure will be valid? Backtracking can be used and it should reduce the time considerably. Given n number of left parentheses and right parentheses, try generate all possible combinations. If the combination turns to be not valid, backtracking can discard it and then try another combination. Let's consider the base case when the number of opening and closing parentheses is equal to n, the number of opening parentheses should be less than n, and a closing parenthesis cannot occur before the open parenthesis.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Instead of adding '(' or ')' every time, let's only add them when we know it will remain a valid sequence. We can do this by keeping track of the number of opening and closing brackets we have placed so far.

```markdown
1) create a list that will store the result
2) call our backtracking function with empty string and initial number of opening and closing parentheses
3) check the base case. If number of opening and closing parentheses are equal to n then we will add the string to the list and return.
4) if the base case does not meet then we will check if number of opening parentheses is less than n, If true, then we will add ( to the current string and increment the count of opening parenthesis.
5) check if number of closing parentheses is less than open parentheses then we will add ) to the current string and increment the count of closing parentheses.
```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

From the problem's description, recursion seems like the way to go. However, this naive approach is to generate all the permutations. All sequence of length n is ( plus all sequences of length n - 1. The time complexity of this will be O(2 to the nth power) which is quite large.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
    def generateParenthesis(self, n):
        """
        :type n: int
        :rtype: List[str]
        """
        OPEN = '('
        CLOSE = ')'
        
        def gen(combo, open_count, close_count, n, res):
            # we have generated all open and close parens, add combo to result
            if open_count == close_count == n:
                # when n == 0
                if combo != '':
                    res.append(combo)
            # used all opens, must close
            elif open_count == n:
                gen(combo + CLOSE, open_count, close_count + 1, n, res)
            # current pairs all closed, must open before another close
            elif open_count == close_count:
                gen(combo + OPEN, open_count + 1, close_count, n, res)
            # otherwise, free to open or close
            else:
                gen(combo + OPEN, open_count + 1, close_count, n, res)
                gen(combo + CLOSE, open_count, close_count + 1, n, res)
  
        res = []
        gen('', 0, 0, n, res)
        return res:
```
```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<String>();
        generateParenthesis(n, n, result, "");
        return result;
    }
    private void generateParenthesis(int left, int right, List<String> result, 
                                    String currString){
        if(left == 0 && right == 0){
            result.add(currString);
            return;
        }
        if(left == right || (left < right && left > 0)){
            // we open a bracket
            generateParenthesis(left - 1, right, result, currString + "(");
            if(left == 0) return;
            if(left == right) return;
            // time to close the remaining opened bracket n - 1 left
            else generateParenthesis(left, right - 1, result, currString + ")");
           
        }
        else if(left == 0 && right > left){
            // we close a bracket
            generateParenthesis(left, right - 1, result, currString + ")");
            if(left == 0) return;
            // time to close the remaining opened bracket n - 1 left
            else generateParenthesis(left, right - 1, result, currString + ")");
        }
        
    }
}

```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.


The complexity analysis based on how many elements there are in generateParenthesis(n).

* **Time Complexity**: This is the n-th [Catalan number](https://en.wikipedia.org/wiki/Catalan_number), where the first Catalan numbers for n = 0, 1, 2, 3, ... are 1, 1, 2, 5, 14, 42, 132, 429, 1430, 4862, 16796, 58786,...
* **Space Complexity**:  n-th Catalan Number
