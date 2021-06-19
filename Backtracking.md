# Backtracking

## Introduction
Backtracking is similar to [[Dynamic Programming]] in that it solves a problem by efficiently performing an exhaustive search over the entire set of possible options. Backtracking is different in that it structures the search to be able to efficiently eliminate large sub-sets of solutions that are no longer possible. For this reason, all backtracking algorithms will have a very similar overall structure for exhaustively searching the space of possible solutions, but the art & difficulty of the particular backtracking solution will be strategies that can be used to get rid of impossible solutions quickly.

## Combination Sum
Let's motivate backtracking by solving the following problem:

> Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers. All numbers will be positive integers. The solution set must not contain duplicate combinations.

### Example 1
```
Input: k = 3, n = 7
Output: [[1,2,4]]
```

### Example 2
```
Input: k = 3, n = 9
Output: [[1,2,6], [1,3,5], [2,3,4]]
```

Backtracking takes solving a problem like this, by breaking it down into solving for 4 components to the algorithm.

1. Given a current sub-solution state, what are all the candidates I can choose to move to a new possible sub-solution state?
2. Decide if all possible solutions derived from the current sub-solution state will be invalid
3. Efficiently backtrack from any current sub-solution state to a previous sub-solution state
4. Is a particular sub-solution actually a valid solution? If so, we can add it to the list of solutions.

Let's examine how to build these pieces for this particular problem and how we weave them together to construct a final solution.

Let's first define a function that will identify valid solutions:
```python
from typing import Set, List  # requires Python 3.6+

def is_valid(s: Set[int], k: int, n: int) -> bool:
  """Return whether `s` is a valid solution."""
  return len(s) == k and sum(s) == n
```

```python
def candidates(s: Set[int], n: int) -> List[int]:
  """
  Generate a list of candidate numbers, any of which could be used to continue the sub-solution `s`.
  The candidate list is the numbers from 1 to 9 that are less than or equal to remaining difference (between `sum(s)` and `n`) and have not been used in `s`.
  """
  rem = n - sum(s)
  return [c for c in range(1, 10) if c <= rem and c not in s]
```

```python
def is_bad(s: Set[int], k: int, n: int) -> bool:
  """
  Return whether `s` is neither a valid solution nor sub-solution.
  Actually, we don't need to use this function because our `candidates` function only generates valid candidates.
  """
  sum_s = sum(s)
  return len(s) > k or sum_s > n or (len(s) == k and sum_s != n) 
```

Now we can create a backtracking search to use these elements to efficiently search the space.

```python
def search(s: Set[int], k: int, n: int, solutions: List[List[int]]) -> None:
  """
  Add solutions to the `solutions` list based on the sub-solution or solution `s`, using backtracking.
  There is actually an issue with this algorithm; it can return the same combination multiple times (just permuted differently).
  """
  if is_valid(s, k, n):
    solutions.append(list(s))  # we have to make a *copy* of `s` to append to `solutions`, because soon after, we will be removing elements from `s`

  for c in candidates(s, n):
    s.add(c)
    search(s, k, n, solutions)
    s.remove(c)

def solve(k: int, n: int) -> List[List[int]]:
  solutions = []
  s = set()
  search(s, k, n, solutions)
  return solutions
```

## Comparison to Dynamic Programming
Dynamic Programming and Backtracking have multiple similarities and differences and are often confused when first learning about them. Often, the confusion comes simply from the name collision where there is a part of DP that involves 'backtracking' to recreate an optimal solution sequence at the end of the DP. For example, when finding the minimum path through a grid in DP we may want to minimize the number of jumps taken, but to reconstruct that final path we often need to go backwards from our final solution to retrace all of the steps that got us to the optimal point in order to reconstruct the minimal path. This is not the Backtracking algorithm.

In backtracking, we are exhaustively searching through a solution space by applying local transformations and collecting solutions we find as we go. It is named 'backtracking' because we will need to back out of each local transformation we make to our current solution in order to continue searching the rest of the space. Imagine searching a labrynth and you have no information so your only choice is to search all paths out. Of course you leave a trail of crumbs behind you so that once you hit a dead end you can backtrack along the crumbs to return to the first position where you could have made a different decision, i.e., all forks in the road. You can continue to do this until you have explored all possible paths through the labrynth.  

It is similar to DP in that we are exhaustively searching a solution space guaranteeing that we will touch all solutions that matter. And it is also similar to DP in that the difference between a highly performant algorithm and not is often how you structure your search. In the case of backtracking, the best solutions often model the problem in some way that allows them to quickly prune state prefixes that cannot lead to solutions.

Unlike DP, backtracking is typically not looking for one optimal solution, but is instead looking for all that satisfy some criteria. So the structure of the algorithm is much less about being efficient at throwing previous information that you don't need because it doesn't lead you to the optimal solution and more about learning what parts of the space you don't need to explore as you are collecting all solutions that qualify.

## Glossary
 * **Candidates** are all the local choices you can make given the current state of the backtracking algorithm.

## References
One of the best discussions of Backtracking can be found in the [Algorithm Design Manual](https://github.com/addyrookie/Depot-App/raw/master/gmail/The%20Algorithm%20Design%20Manual%202ed%20%20by%20Steven%20S.%20Skiena.pdf) by Skiena. The backtracking algorithm is discussed in pages 230 - 244. Highly recommended.
