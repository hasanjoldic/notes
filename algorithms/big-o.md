# Big O Notation

| Growth Rate | Name         |
| ----------- | ------------ |
| 1           | Constant     |
| log(n)      | Logarithmic  |
| n           | Linear       |
| n log(n)    | Linearithmic |
| n^2         | Quadratic    |
| n^3         | Cubic        |
| 2^n         | Exponential  |

## `1` - Constant - statement (one line of code)

```js
a = b + 1;
```

## `log(n)` - Logarithmic - Divide in half (binary search)

```js
while (n > 1) {
  n = n / 2;
}
```

## `n` - Linear - Loop

```js
for (c = 0; c < n; c++) {
  a += 1;
}
```

## `n*log(n)` - Effective sorting algorithms

- Linearithmic
- Mergesort
- Quicksort

## `n^2` - Quadratic - Double loop

```js
for (c = 0; c < n; c++) {
  for (i = 0; i < n; i++) {
    a += 1;
  }
}
```

## `n^3` - Cubic - Triple loop

```js
for (c = 0; c < n; c++) {
  for (i = 0; i < n; i++) {
    for (x = 0; x < n; x++) {
      a += 1;
    }
  }
}
```

## `2^n` - Exponential - Exhaustive search

Trying to braeak a password generating all possible combinations

---

| Name                           | Insert    | Access | Search    | Delete    | Comments                                                                                      |
| ------------------------------ | --------- | ------ | --------- | --------- | --------------------------------------------------------------------------------------------- |
| Array                          | O(n)      | O(1)   | O(n)      | O(n)      | Insertion to the end is O(1).                                                                 |
| HashMap                        | O(1)      | O(1)   | O(1)      | O(1)      | Rehashing might affect insertion time.                                                        |
| Map (using Binary Search Tree) | O(log(n)) | -      | O(log(n)) | O(log(n)) | Implemented using Binary Search Tree                                                          |
| Set (using HashMap)            | O(1)      | -      | O(1)      | O(1)      | Set using a HashMap implementation.                                                           |
| Set (using list)               | O(n)      | -      | O(n)      | O(n)      | Implemented using Binary Search Tree                                                          |
| Set (using Binary Search Tree) | O(log(n)) | -      | O(log(n)) | O(log(n)) | Implemented using Binary Search Tree                                                          |
| Linked List (singly)           | O(n)      | -      | O(n)      | O(n)      | Adding/Removing to the start of the list is O(1).(link-to-details)                            |
| Linked List (doubly)           | O(n)      | -      | O(n)      | O(n)      | Adding/Deleting from the beginning/end is O(1). But, deleting/adding from the middle is O(n). |
| Stack (array implementation)   | O(1)      | -      | -         | O(1)      | Insert/delete is last-in, first-out (LIFO)                                                    |
| Queue (naïve array impl.)      | O(1)      | -      | -         | O(n)      | Remove (Array.shift) is O(n)                                                                  |
| Queue (array implementation)   | O(1)      | -      | -         | O(1)      | Worst time insert is O(n). However, amortized is O(1)                                         |
| Queue (list implementation)    | O(1)      | -      | -         | O(1)      | Using Doubly Linked List with reference to the last element.                                  |
