## Problem Highlights

* 🔗 **Leetcode Link:** <https://leetcode.com/problems/word-pattern/>
* 💡 **Problem Difficulty:** Easy
* ⏰ **Time to complete**: 10 mins
* 🛠️ **Topics**: Hash Table
* 🗒️ **Similar Questions**: [Word Pattern II](https://leetcode.com/problems/word-pattern-ii/)
    
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Does every character map to a single word?
  - Each character has to map to a single word. Every word has to map to a single character.
- Should the time and space complexity analyses ignore the size of words?
  - Since the size (number of entries) of the two hash maps should be the same, it should be O(1). Whenever the number of distinct words goes beyond the number of distinct letters in the pattern, a False value will be returned immediately.


Run through a set of example cases:

```markdown
HAPPY CASE
Input: pattern = "abba", s = "dog cat cat dog"
Output: true

Input: pattern = "abba", s = "dog cat cat fish"
Output: false

EDGE CASE
Input: pattern = "aaaa", s = "dog cat cat dog"
Output: false
```   
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

- Stack: What could we push onto a stack to make this problem easier? Stacks don’t allow us to keep track of data based on keys.
- Queue: Queues fall into the same category as Stacks, do we need to maintain any sense of ordering to solve this problem?
- HashMap: HashMaps allow us to store data for quick access. What could we store in a HashMap to make this problem easier?
- Heap: Do we need some sort of ordering to our data that a Heap could provide?

## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Create two dictionaries that maps words to char and char to word

```markdown
1. For each (character and word) in the lists respectively, check if the mapping of char to word is in the dictionary (and similar for word to char). 
2. If the dictionary doesn’t yet have the mapping, add to it.
3. If at any instance, the mapping doesn’t match, return false. 
4.If after checking every pair, we still match, that means the pattern matches.
```

⚠️ **Common Mistakes**

* Some people may try to approach this problem initially with some brute force O(N^2) strategy. However, this approach doesn’t work. Try to urge students to avoid usual brute force tactics. Instead, urge them to use known data structures to solve this problem.

## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
class Solution:
 def wordPattern(self, pattern: str, s: str) -> bool:
   words = s.split(" ")
   if len(pattern) != len(words):
     return False

   # create two dictionaries charToWord and wordToChar
   wordToChar = {}
   charToWord = {}

   # for each char and word, add them to respective dictionaries
   for char, word in zip(pattern, words):
     # As soon as the mapping doesn’t match, return False
     if char in charToWord and charToWord[char] != word:
       return False
     if word in wordToChar and wordToChar[word] != char:
       return False
     wordToChar[word] = char
     charToWord[char] = word

   # return true if all the pair matches
   return True
```
```java
class Solution {
    public boolean wordPattern(String pattern, String s) {
        // O(n) time | O(n) space
        HashMap<Character, String> myMap = new HashMap<>();
        String[] words = s.split(" ");
        
        // pattern = " a b c" && s = "mice cat dog chicken" then return false directly
        if(pattern.length() != words.length) return false;

        // update myMap in for-loop
        for(int i = 0; i < words.length; i++) {
            char ch = pattern.charAt(i);
             
            if(!myMap.containsKey(ch)) {
                // we need to check the case that, we dont' have such a key in map but value already exists
                // for example, pattern = "abab" && s = "dog dog dog dog"
                if(myMap.containsValue(words[i])) return false;
                
                myMap.put(ch, words[i]);
            }
            else
                if(!myMap.get(ch).equals(words[i]))
                    return false;
        }
        return true;
    }
}
```
    
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assuming that n represents the number of items in the word list or num of characters: 

* **Time Complexity**: `O(n)`
* **Space Complexity**: `O(n)`, required to maintain hashmap.