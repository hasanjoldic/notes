# Abstract data types

## Collection

is a grouping of some *variable* number of data items (possibly zero) that need to be operated upon together. Generally, the data items will be of the same type or, in languages supporting inheritance, derived from some common ancestor type.

Examples of collections include lists, sets, multisets, trees and graphs.

Fixed-size arrays (or tables) are usually not considered a collection because they hold a fixed number of data items, although they commonly play a role in the implementation of collections.  
Variable-size arrays are generally considered collections.

## List

represents a *finite* number of ordered values.

An instance of a list is a computer representation of the mathematical concept of a tuple or finite sequence;.  
The (potentially) infinite analog of a list is a stream.

Lists are typically implemented either as linked lists (either singly or doubly linked) or as arrays, usually variable length or dynamic arrays.

## String

## Set

can store unique values, without any particular order. It is a computer implementation of the mathematical concept of a finite set.

Some set data structures are designed for static or frozen sets that do not change after they are constructed.  
Other variants, called dynamic or mutable sets, allow also the insertion and deletion of elements from the set.

## Multiset

is a special kind of set in which an element can appear multiple times in the set.

## Map

An associative array, map, or dictionary stores a collection of (key, value) pairs, such that each possible key appears at most once in the collection. In mathematical terms, an associative array is a function with finite domain.

The dictionary problem is the classic problem of designing efficient data structures that implement associative arrays. The two major solutions to the dictionary problem are **hash tables** and **search trees**.

> Content-addressable memory is a form of direct hardware-level support for associative arrays.

## Multimap

A multimap (sometimes also multihash, multidict or multidictionary) is a generalization of a map data type in which more than one value may be associated with and returned for a given key. Often the multimap is implemented as a map with lists or sets as the map values.

## Graph

Is meant to implement the undirected graph and directed graph concepts from the field of graph theory within mathematics.

A graph data structure consists of a finite (and possibly mutable) set of **vertices** (also called nodes or points), together with a set of unordered pairs of these vertices for an undirected graph or a set of ordered pairs for a directed graph. These pairs are known as **edges** (also called links or lines), and for a directed graph are also known as edges but also sometimes arrows or arcs.

A graph data structure may also associate to each edge some edge value, such as a symbolic label or a numeric attribute (cost, capacity, length, etc.).

## Tree

Is a widely used abstract data type that represents a hierarchical tree structure with a set of connected nodes. Each node in the tree can be connected to many children (depending on the type of tree), but must be connected to exactly one parent, except for the root node, which has no parent. These constraints mean there are no cycles or "loops" (no node can be its own ancestor), and also that each child can be treated like the root node of its own subtree, making recursion a useful technique for tree traversal.

Trees are commonly used to represent or manipulate hierarchical data in applications such as:

- File systems
- Class hierarchy or "inheritance tree" showing the relationships among classes in object-oriented programming; *multiple inheritance produces non-tree graphs*
- Abstract syntax trees for computer languages
- Natural language processing:
- Document Object Models ("DOM tree") of XML and HTML documents
- Search trees store data in a way that makes an efficient search algorithm possible via tree traversal
- Representing sorted lists of data
- Implementing heaps
- Nested set collections

## Stack

serves as a collection of elements, with two main operations:

- **Push**, which adds an element to the collection
- **Pop**, which removes the most recently added element that was not yet removed

Additionally, a **peek** operation can, without modifying the stack, return the value of the last element added.

The order in which an element added to or removed from a stack is described as last in, first out, referred to by the acronym **LIFO**.

## Queue

Is a collection of entities that are maintained in a sequence and can be modified by the addition of entities at one end of the sequence and the removal of entities from the other end of the sequence.  
By convention, the end of the sequence at which elements are added is called the back, tail, or rear of the queue, and the end at which elements are removed is called the head or front of the queue, analogously to the words used when people line up to wait for goods or services.

The operation of adding an element to the rear of the queue is known as **enqueue**, and the operation of removing an element from the front is known as **dequeue**. Other operations may also be allowed, often including a peek or front operation that returns the value of the next element to be dequeued without dequeuing it.

The operations of a queue make it a first-in-first-out (FIFO) data structure. In a FIFO data structure, the first element added to the queue will be the first one to be removed.

## Priority queue

Similar to a regular queue or stack data structure. Each element in a priority queue has an associated priority. In a priority queue, elements with high priority are served before elements with low priority. In some implementations, if two elements have the same priority, they are served in the same order in which they were enqueued. In other implementations, the order of elements with the same priority is undefined.

## Double-ended queue

Is a queue, for which elements can be added to or removed from either the front (head) or back (tail).

## Double-ended priority queue

A double-ended priority queue (DEPQ) is a data structure similar to a priority queue or heap, but allows for efficient removal of both the maximum and minimum, according to some ordering on the keys (items) stored in the structure.
