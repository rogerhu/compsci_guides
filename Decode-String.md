## Problem Highlights

* 🔗 **Leetcode Link:** [Decode String](https://leetcode.com/problems/decode-string/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 15 mins
* 🛠️ **Topics**: Stack, Array, Recursion
* 🗒️ **Similar Questions**: [Number of Atoms](https://leetcode.com/problems/number-of-atoms/)

## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can there be a nested encoded string?
  - Yes, there can be nested encoded strings like k[string k[string]].
- Is the input always valid?
  - Yes, the input is always valid. 
- How does the pattern start?
  - The pattern begins with a number k, followed by opening braces [, followed by string

   
```markdown
HAPPY CASE
Input: s = "3[a]2[bc]"
Output: "aaabcbc"

Input: s = "3[a2[c]]"
Output: "accaccacc"

Input: s = "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Array problems, we want to consider the following approaches:

- Utilize a Stack (Common Data Structure for Parenthesis Problems)
   - This problem follows the standard pattern for postfix notation. Moreover, to get the output, the expression is evaluated to have basic math with Parentheses like this example: ((2 + 1) * 3) = 9
If you have solved similar problem such as [Evaluate Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/) or [Simplify Path](https://leetcode.com/problems/simplify-path/), it is clear that stack is best suited to implement such problems. We could implement a stack data structure or recursively build the solution by using an internal call stack.
- Queue: Queues fall into the same category as Stacks, do we need to maintain any sense of ordering to solve this problem?
- HashMap: HashMaps allow us to store data for quick access. What could we store in a HashMap to make this problem easier?
- Heap: Do we need some sort of ordering to our data that a Heap could provide?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** The stack follows the Last In First Out (LIFO) Principle, the top of the stack would have the data we must decode.

```markdown
1. Start decoding the last traversed string by popping the string decodedString and number k from the top of the stack.
2. Pop from the stack while the next character is not an opening bracket [ and append each character (a-z) to the decodedString.
3. Pop opening bracket [ from the stack.
4. Pop from the stack while the next character is a digit (0-9) and build the number k.
```

⚠️ **Common Mistakes**

* Firstly, to avoid errors, we return nothing if s is empty. Otherwise, we first figure out how many characters in the beginning of the string are letters and store it in start. If we hit the end of the string, then we just return start since it's all letters.


## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def decodeString(self, s: str) -> str:
        stack = []
        cur_level = []
        num = 0
        
        for char in s:
            if char.isdigit():
                num = num * 10 + int(char)
            
            elif char.isalpha():
                cur_level.append(char)
            
            elif char == '[':
                stack.append((num, [*cur_level]))
                cur_level = []
                num = 0
            
            elif char == ']':
                prev_level_num, prev_level = stack.pop()
                cur_level_string = "".join(cur_level)
                cur_level = [*prev_level, prev_level_num * cur_level_string] 
            
        return "".join(cur_level)
        
```

```java
class Solution {
    public String decodeString(String s) {
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == ']') {
                List<Character> decodedString = new ArrayList<>();
                // get the encoded string
                while (stack.peek() != '[') {
                    decodedString.add(stack.pop());
                }
                // pop [ from the stack
                stack.pop();
                int base = 1;
                int k = 0;
                // get the number k
                while (!stack.isEmpty() && Character.isDigit(stack.peek())) {
                    k = k + (stack.pop() - '0') * base;
                    base *= 10;
                }
                // decode k[decodedString], by pushing decodedString k times into stack
                while (k != 0) {
                    for (int j = decodedString.size() - 1; j >= 0; j--) {
                        stack.push(decodedString.get(j));
                    }
                    k--;
                }
            }
            // push the current character to stack
            else {
                stack.push(s.charAt(i));
            }
        }      
        // get the result from stack
        char[] result = new char[stack.size()];
        for (int i = result.length - 1; i >= 0; i--) {
            result[i] = stack.pop();
        }
        return new String(result);
    }
}

```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the length of output

* **Time Complexity**: `O(N)` since we basically traverse the string and do constant operation on each character
* **Space Complexity**: `O(N)` for the worst case, as the recursion stack can be at most O(n). Think about what the maximum number of pairs of parenthesis could be in a string of size n.