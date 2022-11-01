## Introduction

Stacks and queues are foundational data structures that are useful when adding and removing in particular orders. It is important to be comfortable with these two data structures because depth-first-search and breadth-first-search will use them for graph traversals.

![mae-mu-ecUVGNZA1TM-unsplash](https://user-images.githubusercontent.com/60195141/199291246-082842a7-3182-4b58-836e-a11dc6d60d9d.jpg)

## Stacks
A **stack** is a data structure that stores objects in which the most recently stored objects are the first ones to be removed, (LIFO: last in, first out). An example to help you remember the mechanics of a stack is to associate it with stacks in real life. With a stack of plates, the plates that are placed on top of a stack will be the first ones that are removed from the top!

![](https://i.imgur.com/qMSmxsa.png)

It is important to be comfortable with the common operations of a stack.
* push: the function that is used to add elements into the stack
* pop: the function that is used to remove elements from the stack
* top (peek): a function that returns the first value (what is on top of the stack), but does not remove it
* isEmpty: a function that checks if the stack is empty or not — helpful when trying to clear all the elements from a stack
* size: a function that returns the number of elements that are in a stack at any given time

## Queues
A **queue** is a data structure that stores objects in which the most stored objects are the first ones to be removed. A helpful acronym associated with queues is FIFO, first in first out. An example to help you remember the mechanics of a queue is to associate it with queues in real life. With a queue of people waiting to get a seat in a restaurant, the first people to get in the queue will be the first people seated at that restaurant.

![](https://i.imgur.com/NKuZd0s.png)

It is important to be comfortable with the common operations of a queue.

* enqueue: the function that is used to add elements into the queue
* dequeue: the function that is used to remove the first element from the stack
* peek: a function that returns the first value (what is first in the queue), but does not remove it
* isEmpty: a function that checks if the queue is empty or not — helpful when trying to clear all the elements from a queue
* size: a function that returns the number of elements that are in a queue at any given time

## Key takeaways
* Stacks are useful for backtracking features. For example, parsing questions tend to use stacks because of the LIFO property.
* Stacks can be used to implement recursive solutions iteratively.
* Queues are useful when the ordering of the data matters as it preserves that ordering. For example, they are used for caching.

## Resources
### Guides
* [In depth stacks guide](https://medium.com/basecs/stacks-and-overflows-dbcf7854dc67)
* [In depth queues guide](https://medium.com/basecs/to-queue-or-not-to-queue-2653bcde5b04)

### Libraries
* [Java queue library](https://docs.oracle.com/javase/7/docs/api/java/util/Queue.html)
* [Java stack library](https://docs.oracle.com/javase/7/docs/api/java/util/Stack.html)

* [Python queue library](https://docs.python.org/2/tutorial/datastructures.html#using-lists-as-queues)
* [Python stack library](https://docs.python.org/2/tutorial/datastructures.html#using-lists-as-stacks)
