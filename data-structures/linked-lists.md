# Linked lists

Linked lists are a special way of using classes to create chains of connected objects. Each of these objects contains two pointers, one to the next node and one to the previous node.

While arrays allow you to access an item directly by its index, which is always `O(1)`, our nodes don’t have indexes so we would need to start from the head or tail of the list and use the pointers to search through each item for the one we want to change, so `O(n)`.

Why would we ever want a linked list over an array? Arrays are superior when it comes to _searching_ and _sorting_, because it’s easier to directly access and move items. Linked lists are much more efficient for _inserting_ and _deleting_ items, particularly at the ends. When you insert or remove an item into an array your computer also needs to re-index the rest of the data, giving us `O(n)`, with linked lists can do it in `O(1)` plus the search time, which we have a few ways to optimize.

|                    | Arrays | Linked lists          |
| -----------------: | :----- | :-------------------- |
|          Searching | O(n)   | O(n)                  |
| Inserting/Deleting | O(n)   | Searching time + O(1) |

> Many browsers use a linked list to record your search history, since it’s much more practical for moving up and down a chain when the users rarely need to search for anything or see the whole thing.

```js
class Node {
  constructor(val) {
    this.val = val;
    this.next = null;
    this.prev = null;
  };
};

class linkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.length = 0;
  };
};
```

## Adding

```js
addHead(val) {
  let newNode = new Node(val);

  if (!this.head) {
    this.head = newNode;
    this.tail = this.head;
  };

  this.head.prev = newNode;
  newNode.next = this.head;
  this.head = newNode;

  this.length++;
  return this;
}

addTail(val) {
  let newNode = new Node(val);

  if (!this.head) {
    this.head = newNode;
    this.tail = newNode;
  };

  this.tail.next = newNode;
  newNode.prev = this.tail;
  this.tail = newNode;

  this.length++;
  return this;
}

removeHead() {
  let removed = this.head;
  if (!this.head) return undefined;

  this.head = this.head.next;
  this.head.prev = null;

  this.length--;
  return removed;
}

removeTail() {
  let removed = this.tail;
  if (!this.tail) return undefined;

  if (this.length === 1) {
    this.head = null;
    this.tail = null;
  };

  this.tail = removed.prev;
  this.tail.next = null;

  this.length--;
  return removed;
}
```

## Finding

```js
find(index) {
  let current
  if (index < 0 || index >= this.length) return undefined;

  if (index <= this.length / 2) {
    current = this.head;
    for (let i = 0; i < index; i++) current = current.next;
  } else {
    current = this.tail;
    for (let i = this.length; i > index; i--) current = current.prev;
  }

  return current;
}
```

## Inserting and removing

```js
insert(val, index) {
  if (index < 0 || index > this.length) return null;
  if (index === this.length) return this.addTail(val);
  if (index === 0) return this.addHead(val);

  let prev = this.find(index - 1),
      newNode = new Node(val),
      temp = prev.next;

  prev.next = newNode;
  newNode.next = temp;
  newNode.prev = prev;

  this.length++;
  return true;
}

remove(index) {
  if (index < 0 || index >= this.length) return null;
  if (index === this.length) return this.removeTail();
  if (index === 0) return this.removeHead();

  let removed = this.find(index);

  removed.prev.next = removed.next;
  removed.next.prev = removed.prev;

  this.length--;
  return removed;
}
```

## Updating

```js
update(val, index) {
  let node = this.find(index);
  if (node) node.val = val;
  return node;
}
```