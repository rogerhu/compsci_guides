## Problem Highlights

* 🔗 **Leetcode Link:** [https://leetcode.com/problems/word-ladder/](https://leetcode.com/problems/word-ladder/)
* 💡 **Problem Difficulty:** Hard
* ⏰ **Time to complete**: __ mins
* 🛠️ **Topics**: Graphs
* 🗒️ **Similar Questions**: [Word Ladder II](https://leetcode.com/problems/word-ladder-ii/)

## 1: **U-nderstand**

> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- What are the possible characters in the words?
All lowercase alphabet letters.

- Do all words have the same number of characters?
Yes, all words have the same number of characters.

- Could the beginning or end word be empty?
No, each word must have at least one character.

- Could the beginning word be the same as the ending word?
No, the beginning and ending word will always be different.
    
    ```
    HAPPY CASE
    Input:
       beginWord = "hit"
       endWord   = "cog"
       wordList  = ["hot","dot","dog","lot","log","cog"]
    
    Output: 5
    
    EDGE CASE
    Input:
       beginWord = "hit"
       endWord   = "cog"
       wordList  = ["hot","dot","dog","lot","log"]
    
    Output: 0
    ```
    
## 2: M-atch

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.
    
    For graphs, some of the top things we want to consider are:
    
    - We can use BFS to traverse the graph, which works well for finding optimal path in unweighted graphs.
    - We can use DFS to traverse the graph, but will be less efficient at finding optimal path compared to BFS.
    - We can use an adjacency list to store graph, which works well for our sparse graph.
    - We can use an adjacency matrix to store graph, but will cause runtime slowdowns of O(N^2) for a sparse graph.
    - We can use topological sort to traverse the graph, but will share similar limitations like DFS.

## 3: P-lan
    
> **Plan** the solution with appropriate visualizations and pseudocode.

    Traverse the wordList using BFS, starting from beginWord until we either reach the end or see endWord. As we visit each node, we add all adjacent nodes by looking up if a certain word exists in wordList.
    

```markdown
    1) Create a set of all words in wordList, including endWord.
    2) Create a set of visited nodes
    3) Start BFS from beginWord, setting currentWord
       a) If currentWord is equal to endWord, return number of steps
       b) For each index of currentWord and each letter not equal to currentWord at index
          i)   Create candidateWord equal to currentWord
          ii)  Change candidateWord at current index to new letter
          iii) If it exists and not visited, traverse
    4) If endWord was not found, return 0
    
    Time Complexity: O(N*M^2)
    Space Complexity: O(N*M)
    
```

⚠️ **Common Mistakes**

* Some people may attempt to find adjacent pairs of words by computing finding the number of different letters between each word, whose runtime is O(M^2). Instead, the difference can be computed via iteration over the possible letters to allow a O(M*26) runtime.

## 4: I-mplement

> **Implement** the code to solve the algorithm.
    
```java
    private static final char[] letters = "abcdefghijklmnopqrstuvwxyz".toCharArray();
    
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
      HashSet<String> wordsSet = new HashSet(wordList);
      HashSet<String> visited = new HashSet<>();
      Queue<Pair<String, Integer>> queue = new LinkedList();
      queue.offer(new Pair<>(beginWord, 1));
      while (!queue.isEmpty()) {
       Pair<String, Integer> pair = queue.poll();
       String word = pair.getKey();
       int length = pair.getValue();
       if (word.equals(endWord)) {
          return length;
       }
       for (int i = 0; i < word.length(); i++) {
          for (int j = 0; j < letters.length; j++) {
             if (word.charAt(i) != letters[j]) {
                String candidate = word.substring(0,i) + letters[j] + word.substring(i+1);
                if (wordsSet.contains(candidate) && !visited.contains(candidate)) {
                   queue.offer(new Pair<>(candidate, length + 1));
                   visited.add(candidate);
                }
             }
          }
       }
      }
      return 0;
    }
```
    
```python
    LETTERS = set('abcdefghijklmnopqrstuvwxyz')
    
    def word_ladder(start, end, words):
        words = set(words)
        visited = set()
        queue = [(start, 1)] # queue for BFS, which stores the word and distance
    
        while len(queue) > 0:
            word, length = queue.pop(0)
            if word == end:
                return length
            for i, char in enumerate(word):
                for letter in LETTERS:
                    if char != letter:
                        candidate = word[:i] + letter + word[i + 1:]
                        if candidate in words and candidate not in visited:
                            queue.append([candidate, length + 1])
                            visited.add(candidate)
        return 0
```
    
## 5: R-eview
    
> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errorS and verify the code works for the happy and edge cases you created in the “Understand” section

    
## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Time Complexity: O(M^2 * N), where M is the length of words and N is the total number of words in the input word list
<br>
Space Complexity: O(M^2 * N) to store all MM transformations for each of the NN words in the set or dictionary