# Trees

- have only one root node
- children of one node cannot be its parents nor parents of any ancestor of that node

## Types

### Binary trees

- each node has 0, 1, or 2 children which are referred to as left and right children

### Binary search tree

is a rooted binary tree with the key of each internal node being greater than all the keys in the respective node's left subtree and less than the ones in its right subtree.  
The time complexity of operations on the binary search tree is directly proportional to the height of the tree.

### Self-balancing binary search tree

is a binary search tree that **automatically** keeps its height small in the face of arbitrary item insertions and deletions.

For height-balanced binary trees, the height is defined to be logarithmic in the number of items.

### Red–black tree

is a self-balancing binary search tree.

## Tree traversal

### Breadth-First Search

Breadth-first search is characterized by the fact that it focuses on every item, from left to right, on every level before moving to the next.

### Depth-First Search

Depth-first searches are more concerned with completing a traversal down the whole side of the tree to the leafs than completing every level.

