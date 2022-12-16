# Temp Head Technique
This is a very common linked list technique as it typically saves you creating [special edge condition logic](https://youtu.be/njTh_OwMljA?t=6m35s) in order to operate on the head of a linked list with some algorithms. This technique only involves creating one extra pointer, the temporary head, that will point to your final answer or list that you will return. This technique is much easier to demonstrate with an example.

## Delete Node
Say you are asked to delete a node in a linked list given the value of the node you want to delete. Furthermore you are told that you can assume the values are unique.

For example:
```
Input: 1->2->4, 2
Output: 1->4
```

```python
class Node(object):
    def __init__(self, v):
        self.val = v
        self.next = None
    
def delete_node(head, val):
    d = Node("temp") # 1
    d.next = head
    p = d
    c = head
    while c:
        if c.val == val:
            p.next = c.next # 2
            return d.next   # 3
        p = c
        c = c.next
    return d.next # 4 
```

1. The creation of the temporary head, in this case it is initialized to point to the original list.
2. Because we created the temporary head we don't have to treat deleting the head of the original list any different from other elements in the list.
3. The temporary head points to the answer list so we simply return the next node as the head of the answer.
4. If we don't happen to find the item setting the temporary head to the original list makes it still point to the correct answer.

## Summary
Treating the temporary head with the invariant that it is always pointing to the current correct answer makes dealing with edge cases in linked lists a lot easier and can be found in a number of elegant solutions.

## Reference

* <https://web.archive.org/web/20180227180712/http://www.eternallyconfuzzled.com/tuts/datastructures/jsw_tut_linklist.aspx>