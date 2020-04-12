# 1.3 Bags, Queues, and Stacks

## Bag

A bag is a collection to which items are added, but not removed. It's primarily used to iterate through its contents (without regard for the order of iteration).

### API

- `Bag()` empty bag constructor
- `void add(Item item)` add an item
- `boolean isEmpty()` is the bag empty?
- `int size()` number of items in the bag

### Linked-list Implementation

```java
import java.util.Iterator;

public class Bag<Item> implements Iterable<Item> {

    private Node first;

    private class Node {
        Item item;
        Node next;
    }

    public void add(Item item) {
        Node oldfirst = first;
        first = new Node();
        first.item = item;
        first.next = oldfirst;
    }

    public Iterator<Item> iterator() {
        return new ListIterator();
    }

    private class ListIterator implements Iterator<Item> {
        private Node current = first;

        public boolean hasNext() {
            return current != null;
        }

        public Item next() {
            Item item = current.item;
            current = current.next;
            return item;
        }
    }
}
```

## Queue

A collection based on the FIFO (first in first out) policy. Similar to queues in everyday life, this collection returns items in the same order they were added. A queue is used when the relative order of items needs to be preserved.

### API

- `Queue()` create an empty queue
- `void enqueue(Item item)` add an item
- `Item dequeue()` remove the least recently added item
- `boolean isEmpty()` is the queue empty?
- `int size()` number of items in the queue

### Linked-list Implementation

```java
public class Queue<Item> {
    private Node first; // ref to least recently added node
    private Node last; // ref to most recently added node
    private int n; // number of items in the queue

    private class Node {
        Item item;
        Node next;
    }

    public boolean isEmpty() {
        return first == null;
    }

    public int size() {
        return n;
    }

    public enqueue(Item item) {
        // add new item to the end of the list
        Node oldlast = last;
        last = new Node();
        last.item = item;
        last.next = null;
        if (isEmpty()) {
            first = last;
        } else {
            oldlast.next = last;
        }
        n++;
    }

    public Item dequeue() {
        // remove from the beginning of the list
        Item item = first.item;
        first = first.next;
        n--;
        if (isEmpty()) {
            last = null;
        }
        return item;
    }

    // Iterable implementation is the same as Bag's
}
```

## Stack

A collection based on the LIFO (last in first out) policy. Similar to a stack of documents/plates, the item most recently added to the stack is also the first to be removed. As a result of this policy, a stack is useful when the relative order of inserted items should be reversed.

### API

- `Stack()` create an empty stack
- `void push(Item item)` add an item
- `Item pop()` remove the most recently added item
- `boolean isEmpty()` is the stack empty?
- `int size()` number of items in the stack

### Array-based implementation

Notable features:

- Automatic resizing to accommodate cases where the number of items handled by the collection is unknown
  - Implemented via copying contents from one array to another
  - Array length is doubled when full, halved when under 1/4 capacity
- Iteration
  - Can be used seamlessly with Java's foreach construct by implementing the Iterable interface
- `pop()` implementation avoids loitering by explicitly setting the references of removed items to `null`

```java
import java.util.Iterator // the iterator class is not included in java.lang

public class ResizingArrayStack<Item> implements Iterable<Item> {
    private Item[] items = (Item[]) new Object[1]; // arrays do not support generic instantiation - cast required
    public int n = 0; // number of items in the stack

    public boolean isEmpty() {
        return n == 0;
    }

    public int size() {
        return n;
    }

    public void push(Item i) {
        // resize if array is full
        if (n == items.length) {
            resize(2 * items.length);
        }

        // add item
        items[n++] = i
    }

    public Item pop() {
        // remove item
        Item i = items[--n];

        // remove reference to allow for garbage collection
        items[n] = null

        // resize if array is needlessly large
        if (n > 0 && n == items.length/4) {
            resize(items.length/2);
        }

        return i;
    }

    private void resize(int size) {
        Item[] temp = (Item[]) new Object[size];

        // move items into newly sized array
        for (int i = 0; i < n; i++) {
            temp[i] = items[i]
        }

        items = temp;
    }

    // implement iterable interface
    public Iterator<Item> iterator() {
        return new ReverseArrayIterator();
    }

    private class ReverseArrayIterator implements Iterator<Item> {
        private int i = n - 1;

        public boolean hasNext() {
            return i >= 0;
        }

        // LIFO iteration
        public Item next() {
            return items[i--];
        }

        public void remove() {}
    }
}
```

### Linked-list Implementation

```java
public class Stack<Item> {
    private Node first; // top of stack (most recently added item)
    private int n; // number of items

    private class Node {
        Item item;
        Node next;
    }

    public boolean isEmpty() {
        return first == null;
    }

    public int size() {
        return n;
    }

    public void push(Item item) {
        Node oldfirst = first;
        first = new Node();
        first.item = item;
        first.next = oldfirst;
        n++;
    }

    public Item pop() {
        Item item = first.item;
        first = first.next;
        n--;
        return item;
    }

    // Iterator is the same as Bag's implementation
}
```
