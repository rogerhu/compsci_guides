## Problem Highlights

* 🔗 **Leetcode Link:** <>
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Dynamic Programming
* 🗒️ **Similar Questions**: []()
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input word be blank?
  - No, let’s assume the input word is not empty. However, the input word bank may be empty.
- Can there be multiple valid solutions?
  - Yes, there may be multiple valid solutions. If so, the end result will still be the same, so the specific solution path does not matter much.
   
```markdown
HAPPY CASE
Input: s = "leetcode", wordDict = ["leet","code"]
Output: true

Input: s = "applepenapple", wordDict = ["apple","pen"]
Output: true

EDGE CASE
Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
Output: false
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Two pointer solutions (left and right pointer variables) or a pointer on each string
Since we are not comparing/computing palindromes, the two pointer solution will not give us an effective solution.
- Storing the characters of the string in a HashMap or a Set
In terms of a quick lookup in the word bank, a HashSet makes perfect sense to reduce lookup time from O(N) to O(1)
- Traversing the string with a sliding window
A sliding window of each word in the word bank may help us but will not lead to an efficient solution and may create a ‘hacky’ solution.
- Dynamic Programming
The intuition behind this approach is that the given problem (s) can be divided into subproblems s1 and s2. If these subproblems individually satisfy the required conditions, the complete problem, s also satisfies the same. e.g. "catsanddog" can be split into two substrings "catsand", "dog". The subproblem "catsand" can be further divided into "cats","and", which individually are a part of the dictionary making "catsand" satisfy the condition. 

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Use a DP Array to check if any substring from i -> j is in the word bank. Build up this solution to the last index of the input string.

```markdown
1) Construct a boolean array of size N + 1 as a DP tool
2) Set the first index to True
    - This means there is a valid substring up until that index (exclusive)
3) Iterate from 0 -> N as variable i
    a) If the current index, i, in the DP boolean array is True we iterate from 
        i -> N as variable j
        i) If the substring input[i:j] is within the word bank, we mark DP[j] as true
            - This signifies the substring of the input from 0 -> j (exlusive) is valid
    b) Continue to build the solution to the end
4) The boolean value of the last index signifies whether the input string can 
    be broken

```

**⚠️ Common Mistakes**

* What are some common pitfalls students might have when implementing this solution?

Some people may miss how there are invalid paths that ‘trick’ their program down a certain path while there were also other valid words in the bank. Usually, this is due to premature matching and no ‘recursive’ efforts.


## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def wordBreak(self, word: str, wordList: List[str]) -> bool:
        wordSet = set(wordList) #O(M)
        dp = [True] + [False] * len(word)
        for i in range(len(word)): # O(N)
            if dp[i]:
                for j in range(i + 1, len(word) + 1): # O(N)
                    if word[i:j] in wordSet: # O(N) String Generation
                        dp[j] = True     
        return dp[-1]
```
```java
public class Solution {
    public boolean wordBreak(String s, List<String> wordDict) {
        Set<String> wordDictSet = new HashSet<>(wordDict);
        boolean[] dp = new boolean[s.length() + 1];
        dp[0] = true;
        for (int i = 1; i <= s.length(); i++) {
            for (int j = 0; j < i; j++) {
                if (dp[j] && wordDictSet.contains(s.substring(j, i))) {
                    dp[i] = true;
                    break;
                }
            }
        }
        return dp[s.length()];
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

n is the length of the input string.

* **Time Complexity**: O(n^3), as there are two nested loops, and substring computation at each iteration.
* **Space Complexity**: O(n)