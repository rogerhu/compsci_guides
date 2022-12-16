## Problem Highlights

* 🔗 **Leetcode Link:** [Word Search II](https://leetcode.com/problems/word-search-ii)
* 💡 **Difficulty:** Hard
* ⏰ **Time to complete**: 60 mins
* 🛠️ **Topics**: 2D-Array, Trie Node, Backtracking, DFS
* 🗒️ **Similar Questions**: [Word Search](https://leetcode.com/problems/word-search/), [Unique Paths III](https://leetcode.com/problems/unique-paths-iii/), [Encrypt and Decrypt Strings](https://leetcode.com/problems/encrypt-and-decrypt-strings/)

## 1: U-nderstand
 
> **Understand** what the interviewer is asking for by using test cases and questions about the problem.
> 
> - Established a set (2-3) of test cases to verify their own solution later.
> - Established a set (1-2) of edge cases to verify their solution handles complexities.
> - Have fully understood the problem and have no clarifying questions.
> - Have you verified any Time/Space Constraints for this problem?

- Can the input board or word list be blank?

    - Let’s assume the current word list and board are not blank. We don’t need to consider empty or Null inputs.
- Does the direction in which the word is uncovered on the board matter?
    - Nope. The word can be written out in four directions, even backward, on the board.
```markdown
HAPPY CASE
Input: board = [
["o","a","a","n"],
["e","t","a","e"],
["i","h","k","r"],
["i","f","l","v"]], 
words = ["oath","pea","eat","rain"]
Output: ["eat","oath"]

Input: board = [
[["g", "i", "z"],
["a", "u", "t"],
["q", "s", "e"]
]
words = ["quest", "for", "zig"]
Output: ["zig"]

EDGE CASE (Empty String)
Input: board = [
["a","b"],
["c","d"]], 
words = ["abcb"]
Output: []
```   
    
## 2: M-atch

<!-- See https://docs.google.com/document/d/1hYT1hoOJ6pFIt8A5q-PIZmYP7pB4WqlzyUJgFx9x2mY/edit#heading=h.ya2de4n4zsds for list of algorithms based on question type-->

> **Match** what this problem looks like to known categories of problems, e.g. Linked List or Dynamic Programming, and strategies or patterns in those categories.

For 2D-Array, common solution patterns include:

- Perform a BFS/DFS Search through the 2D Array
    - DFS: Our goal is to find if the words exists in the matrix or not. We only have to look at the adjacent cells (ignore the diagonal ones). Match character-by-character, go ahead and check if the adjacent cells match the next character, and go back if it does not match. How should we traverse the matrix efficiently? We need to think of a traversal approach. BFS? DFS? Both can work. But DFS will be better as it immediately checks the next position and returns if it is not needed after marking it as visited.
 
- Hash the 2D Array in some way to help with the Strings
    - We do need a visited hashset to ensure that we do not revisit the same position twice.
    
- Create/Utilize a Trie
    - A Trie is needed for this problem, to speed up the code and seek for multiple words with one DFS call. I.E. oath and oaths, once we find oath, we want to continue the path and not start over again searching the entire grid from square 0,0. 


## 3: P-lan

> **Plan** the solution with appropriate visualizations and pseudocode.

**General Idea:** Build a trie node dictionary of all the words. As we start at each position from the grid, we can check for path that exist in both the grid and trieNode. Once all paths are exhausted, we can stop and return the words found. 

```markdown
1. Build TrieNode to hold each character of each word.
    a. Initialize children and isWord
    b. Define addWord
        i. set curr node to root node 
        ii. for each character in word: build trieNode tree set curr.children 
        iii. set last node isWord to True
    c. Define pruneWord: remove word from tree, so we don't reseek the same path once word was found.
        i. set curr node to root node
        ii. initialize parentNode to child key(next character) list
        iii. for each character in word. add current node and child key(next character)
        iv. for each parentNode and character in word in reverse check if it has any children
            a. if not delete the key and repeat
2. Initialize solution board, visited, found, and root of TrieNode
3. Define dfs to build the words available on board and in trieNode, and collect found words
    a. Check if position is available 
        i. out of bound
        ii. next character in TrieNode
        iii. position not in visited
    b. Position is available 
        i. Build word with next character
        ii. Move trieNode to next character
        iii. Check if trieNode is Word
        iv. If word: append to found and pruneWord
        v. Add position to visited
        vi. Continue check down four paths
        vii. Backtrack and remove position from visited
4. Define findWords to check each start position against our TrieNode paths 
    a. Build TrieNode paths with all words with less characters than board to TrieNode
    b. Run dfs from each start point in the board against our TrieNode to find words.
    c. Return found words
```

**⚠️ Common Mistakes**

* Some people might forget on using a visited set or some mechanism to keep track of what indices have already been used in creating the current word.


## 4: I-mplement

> **Implement** the code to solve the algorithm.

```python
# Build TrieNode to hold each character of each word.
class TrieNode:
    # Initialize children and isWord
    def __init__(self):
        self.children = defaultdict(TrieNode)
        self.isWord = False
    
    # Define addWord
    def addWord(self, word):
        # set curr node to root node 
        curr = self
        # for each character in word: build trieNode tree set curr.children 
        for char in word:
            curr = curr.children[char] 
        # set last node isWord to True
        curr.isWord = True
    
    # Define pruneWord: remove word from tree, so we don't reseek the same path once word was found. 
    def pruneWord(self, word):
        # set curr node to root node
        curr = self

        # initialize parentNode to child key(next character) list
        parentNodeToChildKey: list[tuple[TrieNode, str]] = []
        
        # for each character in word. add current node and child key(next character)
        for char in word:
            parentNodeToChildKey.append((curr, char))
            curr = curr.children[char]
        
        # for each parentNode and character in word in reverse check if it has any children
        for parentNode, childKey in parentNodeToChildKey[::-1]:
            targetNode = parentNode.children[childKey]
            # if not delete the key and repeat
            if len(targetNode.children) == 0:
                del parentNode.children[childKey]
            else:
                return
       
class Solution:
    # Initialize solution board, visited, found, and root of TrieNode
    def __init__(self):
        self.board = []
        self.visited = set()
        self.found = []
        self.root = TrieNode()
        
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:
        m, n = len(board), len(board[0])
        self.board = board
        # Build TrieNode paths with all words with less characters than board to TrieNode
        for word in [word for word in words if len(word) <= m * n]:
            self.root.addWord(word)
        
        # Run dfs from each start point in the board against our TrieNode to find words.
        for i in range(m):
            for j in range(n):
                self.dfs(i, j, "", self.root)
        
        # Return found words
        return self.found

    # Define dfs to build the words available on board and in trieNode, and collect found words
    def dfs(self, r: int, c: int, buildWord:str, trieNode: TrieNode):
        # Check if position is available, out of bound, next character in TrieNode, position not in visited
        m, n = len(self.board), len(self.board[0])
        if (r < 0 or r == m or
            c < 0 or c == n or
            self.board[r][c] not in trieNode.children or
            (r,c) in self.visited):
            return 

        # Position is available 
        # Build word with next character
        # Move trieNode to next character
        char = self.board[r][c]
        buildWord += char
        trieNode = trieNode.children[char]
        
        # Check if trieNode is Word -> If word: append to found and pruneWord
        if trieNode.isWord:
            self.found.append(buildWord)
            trieNode.isWord = False
            self.root.pruneWord(buildWord)
        
        # Add position to visited
        self.visited.add((r,c))

        # Continue check down four paths
        self.dfs(r+1, c, buildWord, trieNode) 
        self.dfs(r-1, c, buildWord, trieNode) 
        self.dfs(r, c + 1, buildWord, trieNode) 
        self.dfs(r, c - 1, buildWord, trieNode)

        # Backtrack and remove position from visited
        self.visited.remove((r,c))
```

## 5: R-eview

> **Review** the code by running specific example(s) and recording values (watchlist) of your code's variables along the way.

- Trace through your code with an input to check for the expected output
- Catch possible edge cases and off-by-one errors

## 6: E-valuate

> **Evaluate** the performance of your algorithm and state any strong/weak or future potential work.

Assume `N` represents the number of rows in 2D-array.
Assume`M` represents the number of columns in 2D-array.
Assume `W` represents the number of words in array.
Assume `L` represents the number of characters in the word.

* **Time Complexity**: `O(N*M*W*L*3)` because we need to start at each point in the grid, we may have unique words that do not share common characters with other words, and we need to traverse each character of the word with 3 DFS calls.
* **Space Complexity**: `O(W*L)` because we need to create a Trie Node for each character of each word.