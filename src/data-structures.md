# Data Structures

## Be comfortable with the purpose and functionality of each of the following data structures

1. Stack
2. Queue
3. Binary Heap
4. Binary Search Trees
5. Graphs (covered separately)

For each data structure below you'll find:

- What it is and when to use it
- Time and space complexity for core operations
- Notable properties and implementation details
- Practical use cases and trade-offs

---

## Stack

### What it is

A linear data structure that follows the **Last-In-First-Out (LIFO)** principle: the most recently added element is the first one removed. Think of a stack of plates — you add and remove from the same end (the top).

### Core Operations & Complexity

- **Push** (insert): O(1) amortized
- **Pop** (remove): O(1)
- **Peek** (view top): O(1)
- **Space:** O(n) where n is the number of elements

### Notable Features

- Stacks are typically implemented as a **dynamic array (vector)** in most languages, including Rust.
  - Elements are stored contiguously in memory.
  - Push appends to the end; pop removes from the end.
  - Amortized O(1) because the vector occasionally reallocates, but this happens infrequently.
  
- Stacks are "just a vec" (or array) because:
  - You only ever access/modify one end, so you don't need fancy data structures.
  - The vector's end pointer and capacity handle the abstraction efficiently.
  - No need for linked lists — contiguous memory is faster in practice due to cache locality.

### When to Use

- Function call stacks (recursion, system call frames)
- Expression evaluation (infix to postfix conversion)
- Backtracking algorithms (DFS uses an implicit or explicit stack)
- Parenthesis matching and bracket validation

### Example Walkthrough

Stack operations: Push 5, Push 3, Push 7, Pop, Peek, Pop

- After Push 5: [5]
- After Push 3: [5, 3]
- After Push 7: [5, 3, 7]
- After Pop: [5, 3], returned 7
- After Peek: [5, 3], return 3 (no change)
- After Pop: [5], returned 3

> In rust, all peek and pop operations return `Option<T>`, to account for the possibility of an empty stack (same goes for Queues)

---

## Queue

### What it is

A linear data structure that follows the **First-In-First-Out (FIFO)** principle: elements are inserted at the back and removed from the front, like a line at a store.

### Core Operations & Complexity

- **Enqueue** (insert back): O(1) amortized
- **Dequeue** (remove front): O(1) amortized
- **Peek** (view front): O(1)
- **Space:** O(n) where n is the number of elements

### Notable Features

- **Why it isn't just a vec:**
  - If you use a simple dynamic array, dequeuing from the front is O(n) because you'd need to shift all remaining elements forward.
  - A standard vector only provides efficient removal from the end.

- **Circular Buffer / VecDeque (Rust's efficient queue):**
  - Uses a **circular buffer**: a fixed-size array where the "front" and "back" pointers wrap around.
  - Both enqueue and dequeue operate at pointers without shifting, achieving O(1) amortized.
  - When the circular buffer is full, it reallocates (like a vector) to a larger size.
  - Memory usage is linear O(n), but the buffer may have some wasted capacity between reallocations.
  
- **Why not a linked list?**
  - Linked lists waste memory on pointers (worse cache locality).
  - Circular buffers are more cache-friendly and faster in practice, even with potential reallocations.
  - Rust's `VecDeque` uses a circular buffer for this reason.

> You can either use `push_back` and `pop_front` OR `push_front` and `pop_back`, but NOT `push_front` and `pop_front` to implement a Queue. Why?

### When to Use

- BFS traversal (visit nodes level-by-level)
- Task scheduling (jobs queued for processing)
- Print job management

### Example Walkthrough

Queue operations: Enqueue 10, Enqueue 20, Enqueue 30, Dequeue, Peek, Dequeue

- After Enqueue 10: [10]
- After Enqueue 20: [10, 20]
- After Enqueue 30: [10, 20, 30]
- After Dequeue: [20, 30], returned 10
- After Peek: [20, 30], return 20 (no change)
- After Dequeue: [30], returned 20

---

## Binary Heap

### What it is

A **complete binary tree** (all levels filled except possibly the last, which is filled left-to-right) that satisfies the **heap property**:

- **Min-Heap:** parent ≤ children
- **Max-Heap:** parent ≥ children

Used as an efficient priority queue data structure.

### Tree Representation

Conceptually, a binary heap is visualized as a tree:

```
Min-Heap Example:
        1
       / \
      3   5
     / \ /
    7  6 8
```

### Array Representation

Heaps are stored in an **array** for efficiency:

- Root at index 0
- For node at index i:
  - Left child at index 2i + 1
  - Right child at index 2i + 2
  - Parent at index ⌊(i−1)/2⌋

Same heap as above in array form: `[1, 3, 5, 7, 6, 8]`

### Core Operations & Complexity

- **Insert:** O(log n)
- **Delete (extract min/max):** O(log n)
- **Peek (view min/max):** O(1)
- **Space:** O(n)

### Insert Procedure (Sift-Up / Bubble-Up)

1. Add the new element to the end of the array (next available leaf position).
2. **Sift up:** Compare with parent; if violates heap property, swap with parent.
3. Continue up the tree until the heap property is restored or you reach the root.

**Example (min-heap):** Insert 2 into `[1, 3, 5, 7, 6, 8]`

- Add 2 at end: `[1, 3, 5, 7, 6, 8, 2]`
- 2's parent (index 2) is 5; 2 < 5, so swap: `[1, 3, 2, 7, 6, 8, 5]`
- 2's parent (index 1) is 3; 2 < 3, so swap: `[1, 2, 3, 7, 6, 8, 5]`
- 2's parent (index 0) is 1; 2 > 1, so stop.
- Result: `[1, 2, 3, 7, 6, 8, 5]`. Heap property maintained.

### Remove (Extract Min/Max) Procedure (Sift-Down / Bubble-Down)

1. Extract the root (min or max).
2. Move the **last element** to the root position.
3. **Sift down:** Compare the root with its children; if it violates the heap property, swap with the smaller (min-heap) or larger (max-heap) child.
4. Continue down until the heap property is restored or you reach a leaf.

**Example (min-heap):** Remove min from `[1, 2, 3, 7, 6, 8, 5]`

- Extract 1; move last element (5) to root: `[5, 2, 3, 7, 6, 8]`
- 5's children are 2 and 3; min is 2; 5 > 2, so swap: `[2, 5, 3, 7, 6, 8]`
- 5's children are 7 and 6; min is 6; 5 < 6, so stop.
- Result: `[2, 5, 3, 7, 6, 8]`. Heap property maintained.

### When to Use

- Priority queues (e.g., Dijkstra's algorithm, Huffman coding)
- Heap sort (in-place sorting algorithm)
- Finding the k smallest/largest elements
- Scheduling and load balancing
- Median finding in a stream

### Notable Properties

- Efficient for repeated access to the minimum (or maximum) element.
- Not suitable for arbitrary element lookup or deletion (beyond the root).
- Both insert and remove are O(log n), making heaps very efficient for dynamic min/max queries.

---

## Binary Search Tree (BST)

### What it is

A **binary tree** where each node has at most two children and satisfies the **BST property**:

- All values in the left subtree are **less than** the node's value.
- All values in the right subtree are **greater than** the node's value.

This ordering enables efficient searching and insertion.

### Core Operations & Complexity

- **Search:**
  - Best/Average: O(log n) (balanced tree)
  - Worst: O(n) (degenerate/linear tree if not balanced)
  
- **Insert:**
  - Best/Average: O(log n)
  - Worst: O(n) (if tree becomes unbalanced)
  
- **Delete:**
  - Best/Average: O(log n)
  - Worst: O(n)

- **In-order traversal:** O(n) (yields elements in sorted order)
- **Space:** O(n)

### Notable Features

- **Ordered traversal:** In-order DFS on a BST yields elements in sorted order.
- **Balancing problem:** A naive BST can degrade into a linked list if insertions are in sorted order (e.g., 1, 2, 3, 4, 5).
  - Solution: **Self-balancing BSTs** (AVL, Red-Black trees) maintain O(log n) height through rotations.
  - Rust's standard library uses **red-black trees** for `BTreeMap` and `BTreeSet`.

### Search Procedure

1. Start at root.
2. Compare target with current node:
   - If equal, found it.
   - If target < current, go left.
   - If target > current, go right.
3. Continue until found or reach a null pointer (not found).

**Example:** Search for 6 in BST with root 4, left child 2, right child 6

- Compare 6 with 4: 6 > 4, go right.
- Found 6.

### When to Use

- Dictionaries and maps (though hash tables are often faster for lookup alone).
- Sorted data structures (maintaining sorted order with dynamic insertions/deletions).
- Range queries (find all elements between a and b).
- Building decision trees.
- Self-balancing variants (AVL, Red-Black) are used in databases and filesystems.

### Balanced vs. Unbalanced

- **Balanced BST:** height ≈ log n — all operations are O(log n).
- **Unbalanced BST:** height up to n — degenerates to a linked list if not managed.
- **Self-balancing:** Red-Black trees and AVL trees automatically rebalance on insertion/deletion through rotations, guaranteeing O(log n) operations.

---

## BTreeMap / BTreeSet (brief)

### What they are

`BTreeMap` and `BTreeSet` are ordered map/set implementations based on a B-tree (a balanced, multi-way search tree). Unlike binary trees, B-trees keep multiple keys per node and keep the tree shallow by allowing many children per node.

### Complexity and when to use

- **Search/Insert/Delete:** O(log n) (log base depends on node fanout)
- **Space:** O(n) with extra per-node overhead for storing multiple keys
- Use `BTreeMap`/`BTreeSet` when you need an ordered map/set with good locality and predictable O(log n) performance. For example, you can have a HashMap with sorted keys (alphabetical order)!

### Practical notes

- Iteration over a `BTreeMap` or `BTreeSet` yields elements in sorted order without additional work.
- B-trees are especially useful when nodes are sized to match cache lines or disk pages (reducing pointer chasing). You'll learn more about this in 310!

## Summary Table

| Data Structure | Insert | Remove | Search | Use Case |
|---|---|---|---|---|
| Stack | O(1) | O(1) | O(n) | LIFO, recursion, undo |
| Queue | O(1) | O(1) | O(n) | FIFO, BFS, task scheduling |
| Binary Heap | O(log n) | O(log n) | O(n) | Priority queues, heapsort |
| BST (balanced) | O(log n) | O(log n) | O(log n) | Sorted maps, range queries |
| BST (unbalanced) | O(n) | O(n) | O(n) | Bad — avoid in practice |
