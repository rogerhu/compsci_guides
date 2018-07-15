# Multiple Pass Technique
Most computations on a list will require O(N) time complexity, so a simple but very useful technique is to pass through the list a constant number of times to calculate some summary of the list that will simplify your algorithm. One example that we see a lot is the need to calculate the length of the list. That sounds simple enough, but let's see an example to motivate this technique better.

## Example: Intersection of Two Linked Lists
Let's take a look at this common problem as defined in [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists).
> Write a program to find the node at which the intersection of two singly linked lists begins.
For example, the following two linked lists:
```
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3
```
begin to intersect at node c1.

>Notes:
- If the two linked lists have no intersection at all, return null.
- The linked lists must retain their original structure after the function returns.
- You may assume there are no cycles anywhere in the entire linked structure.
- Your code should preferably run in O(n) time and use only O(1) memory.

We will not cover the full solution to this problem here, but instead cover two insights that can make the solution much simpler. The first insight is that once a list has intersected with another list the rest of the list is identical. Next, a list can only be identical if it is the same length. With these insights a strawman solution is to ignore any prefix of the longer of the two lists because it can't be part of the intersection. The rest of the algorithm also flows much easier after that. So let's just take the sub-task of taking two lists of any length and returning references to two lists of the same length. In the example above it would be lists starting at `a1` and `b2`.

```python
class Node(object):
    def __init__(self, v):
        self.val = v
        self.next = None

    def __repr__(self):
        return f"{self.val} --> {self.next}"

    def insert(self, v):
        n = Node(v)
        n.next = self
        return n

def get_len(ll):
    l = 0
    while ll:
        l += 1
        ll = ll.next
    return l

def make_same_length(ll1, ll2):
    len1 = get_len(ll1)
    len2 = get_len(ll2)
    if len1 > len2:
        long_ll, short_ll = ll1, ll2
        long_len, short_len = len1, len2
    else:
        long_ll, short_ll = ll2, ll1
        long_len, short_len = len2, len1
    while long_len > short_len:
        long_len -= 1
        long_ll = long_ll.next
    return short_ll, long_ll

common = Node('c1')
short = common.insert('a2').insert('a1')
long = common.insert('b3').insert('b2').insert('b1')
make_same_length(short, long)
```

### Output:
```python
"(a1 --> a2 --> c1 --> None, b2 --> b3 --> c1 --> None)"
```

### Aside: Recursion vs. Iteration
Let's take an opportunity to use the length calculation to demonstrate recursive vs. iterative solutions. Recursive algorithms are often very easy to write with linked lists because the list is structured recursively.

```python
def get_len_recur(ll):
    if not ll: return 0 # A None path has 0 length
    return get_len_recur(ll.next) + 1 # The length at this node is 1 + length of rest

def get_len_iter(ll):
    l = 0
    while ll:
        l += 1
        ll = ll.next
    return l

get_len_recur(long), get_len_iter(long)
```

#### Output:
```python
(4, 4)
```

The recursive length calculation has three parts to the function:
1) What to return in the "base" case? I.e., the None value has length 0
2) Given any node of the path, how to calculate the length of the path from that node until the end?
3) How to relate the desired answer to the final recursive computation? In this case, it is exactly equal to the final recursive computation so we just return the computation.

The recursive algorithm reads more simply because the looping is not explicitly written, i.e., the `while ll` line in the iterative algorithm. Unfortunately for recursion, this is also the major drawback of the recursive algorithm. The looping is implicitly done for you by the execution of your program, namely every successive call to your recursive function places some data on your call stack and then goes to the next function. For most situations, this means you are paying a storage penalty equal to the size of the list. Some languages can get around this with [tail call](https://en.wikipedia.org/wiki/Tail_call) optimization.