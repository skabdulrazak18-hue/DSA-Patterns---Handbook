

# Data Structure: Queue

### Goal

Efficiently manage elements in **FIFO** (First-In, First-Out) order to handle tasks, data streaming, or sequential processing.

---

## 1. Core Idea

A **Queue** is a linear data structure that operates on the **FIFO** principle. Elements are added to the back (`enqueue`) and removed from the front (`dequeue`). It behaves exactly like a line at a store: the person who arrives first is served first.

### Theory

By restricting access to only the front and back, the queue preserves the chronological order of data, ensuring that we process items in the exact sequence they were received.

---

## 2. Why Queue Matters

1. **Sequential Processing:** Naturally handles tasks in the order they arrive.
2. **Buffering:** Acts as a bridge between processes that produce and consume data at different speeds.
3. **Level-Order Logic:** Essential for exploring search spaces layer by layer.
4. **Efficiency:** Provides $O(1)$ time for both insertion and removal, keeping processing overhead minimal.

---

## 3. When to Use

### Checklist

* You need to process tasks in the exact order they arrive.
* You are implementing a search algorithm that visits neighbors level by level (BFS).
* You need to buffer asynchronous data (e.g., printer queues, task scheduling).

### Key Phrases to Watch For

* "Process tasks in the order received..."
* "Level-order traversal..."
* "Shortest path in an unweighted graph..."
* "Implement a buffer..."

---

## 4. Typical Elements of the Pattern

### A. The Container

Usually a `std::queue` (C++), `java.util.Queue`, or `collections.deque` (Python).

### B. Enqueue

Adding an element to the back of the queue.

### C. Dequeue

Removing the oldest element from the front of the queue.

### D. Front/Peek

Viewing the element at the front without removing it.

---

## 5. Common Variations

| Variation | Common Applications |
| --- | --- |
| **Simple FIFO** | Job scheduling, printing, handling inputs. |
| **Circular Queue** | Fixed-size buffers where space is reused (e.g., IO buffers). |
| **Priority Queue** | Dijkstra’s algorithm, task priorities. |
| **Deque** | Double-ended operations (adding/removing from both sides). |
| **Monotonic Queue** | Sliding Window Maximum problems. |

---

## 6. Recommended Learning Order

Simple FIFO
↓
Breadth-First Search (BFS)
↓
Circular Queue
↓
Deque
↓
Monotonic Queue
↓
Sliding Window Maximum

---

## 7. Time and Space Complexity

| Operation | Time Complexity | Space Complexity |
| --- | --- | --- |
| Enqueue / Dequeue | $O(1)$ | $O(n)$ |

---

## 8. How to Identify

If a problem requires processing items in their original order of arrival or exploring a space layer by layer, a **Queue** is often the intended data structure.

---

## 9. Important Note

### Queue Philosophy

* **Stacks** prioritize the most recently added element (LIFO).
* **Queues** prioritize the oldest element (FIFO).
* **Stack** → LIFO → Depth-first behavior.
* **Queue** → FIFO → Breadth-first behavior.

This distinction is crucial when moving from simple data structures to Tree and Graph traversals.

---

## 10. Related Structures

Queue
↓
Deque
↓
Monotonic Queue
↓
Sliding Window Problems
---------------------------------------------------------
---

