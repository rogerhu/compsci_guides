## Problem Highlights

* 🔗 **Geeksforgeeks Link:**  [Word Frequency](https://www.geeksforgeeks.org/count-occurrences-of-a-word-in-string/)
* 💡 **Difficulty:** Medium
* ⏰ **Time to complete**: 20 mins
* 🛠️ **Topics**: Array, Stack
* 🗒️ **Similar Questions**: [Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/), [Sender With Largest Word Count](https://leetcode.com/problems/sender-with-largest-word-count/)
## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Could my inputs ever be null or empty?
    - Yes. Return a value of -1 in these cases.
- Does case-sensitivity matter?
    - No. For example, ‘puppy’ and ‘Puppy’ should not be counted as two separate words in the book.
- Can I assume that the input characters in the String will only be alphabetical letters?
    - No. You don’t need to handle unexpected symbols in the input, but a string could potentially have a space in it. So for example, ’ queen ’ and ‘queen’ should NOT be counted as two different words in the book just because one has extra spaces.
- Should I make my method time / space efficient?
    - Yes, make sure it can efficiently handle multiple calls.
```markdown
HAPPY CASE
Input: [" The", "dog", "jumped", "in", "the", "dog", "house"], "dog"
Output: 2

Input: [" The", "dog", "jumped", "in", "the", "dog", "house"], "house"
Output: 1
 
 
EDGE CASE
Input: [], "house"
Output: -1
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For Strings/Arrays, some things we should consider are:

- Sort. Sorting isn't very effective for counting here.. 
- Two pointer solutions (left and right pointer variables). Two pointer solution doesn't help us count the frequency of the word.
- Storing the elements of the array in a HashMap or a Set. A HashMap would be great for counting each time a word is seen.
- Traversing the array with a sliding window. We are not restricted to a window size for a count.


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Look through each element in the array and increment a counter every time it matches the given word.

```markdown
1. Check for edge case where inputs are null, if so return -1.
2. Create a variable count and set to 0. 
3. Loop through every string s in the book (array of strings).
  1. Convert the string s to all lowercase, to remove case sensitivity.
  2. If the string s is equal to the given word, then increment count by 1
  3. Else, do nothing.
4. Return count.
```

**⚠️ Common Mistakes**

* Look out to pop and push element to the right Stack
## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
def word_freq(book, word):
    # Check edge case if book is None or empty array/list
    if book is None or word is None or len(book) == 0 or word == "":
        return -1

    # Initialize a counter to count all occurrences of the word
    count = 0

    # Loop through every string in the book
    for s in book:
        # Convert string to all lowercase to remove case sensitivity
        s = s.lower()
        # Check to see if s is equal to the word.
        # We also convert the word to lowercase to remove case sensitivity.
        if s == word.lower():
            # If the element in the book and word are equal, increase counter
            count += 1
        # Else, do nothing
    return count
```
```java
static int wordFreq(String[] book, String word)
{
 // Check edge case if either parameter is null or empty
 if (book == null || word == null || book.length == 0 || word.equals("")) {
  return -1;
 }

 // Initialize a counter to count all occurrences of the word
 int count = 0;
	
 // Loop through every string in the book
 for (String s : book) {
  // Convert string to all lowercase to remove case sensitivity
  s = s.toLowerCase();
  // Check to see if s is equal to the word.
  // We also convert the word to lowercase to remove case sensitivity.
  if (s.equals(word.toLowerCase())) {
   // If the element in the book and word are equal, increase counter
   count++;
  }
  // Else, do nothing
 }

 return count;
}
```
## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of elements in the array.

* **Time Complexity**: `O(N)` because we will need to access each word in the array.
* **Space Complexity**: `O(N)` because we may need to store all items in the array.