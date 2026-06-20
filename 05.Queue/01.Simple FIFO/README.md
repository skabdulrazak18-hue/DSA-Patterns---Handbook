
# Pattern Name: Simple FIFO (First-In, First-Out)

### Goal

Implement a basic queue structure to process data in the exact order it arrives, ensuring $O(1)$ operations for both insertion and removal.

---

## 1. Core Idea

The **Simple FIFO** queue acts as a linear buffer. Elements enter at the `back` (`enqueue`) and exit from the `front` (`dequeue`). This ensures strict chronological order.

### Theory

The queue uses two primary pointers (or references):

* **Front:** Points to the oldest element (next to be removed).
* **Back:** Points to the newest element (next to be inserted).

---

## 2. Why Simple FIFO Matters

1. **Ordering:** Guarantees that the first task accepted is the first task completed.
2. **Predictability:** Eliminates the complexity of reordering or prioritizing elements.
3. **Foundation:** Serves as the base structure for all complex queue-based algorithms, including BFS and task schedulers.

---

## 3. When to Use

### Checklist

* You need to maintain the sequence of incoming data.
* You are buffering data for asynchronous processing.
* You are implementing a basic task scheduler.

### Key Phrases to Watch For

* "Process tasks in the order received..."
* "Store logs chronologically..."
* "First come, first served..."

---

## 4. Typical Elements of the Pattern

### A. The Container

An array or linked list, accessed exclusively through `enqueue` and `dequeue` methods.

### B. Size/Capacity

Tracked to manage memory allocation and prevent overflow/underflow.

---

## 5. Applications

### Sequential Processing

* Task Scheduling
* Printer Queue
* Log Processing

### Buffering

* Producer–Consumer Systems
* Streaming Data
* IO Buffers

### Foundation For

* Breadth-First Search (BFS)
* Level Order Traversal
* Multi-Source BFS

---

## 6. Template (Using `std::queue`)

```cpp
#include <queue>
using namespace std;

void fifoExample() {
    queue<int> q;

    // Enqueue: Add element to back
    q.push(10);
    q.push(20);

    // Dequeue: Remove oldest element from front
    while (!q.empty()) {
        int front = q.front();
        q.pop(); 
    }
}

```

---

## 7. Common Pitfalls to Avoid

1. **Empty Queue Access:** Always check `!q.empty()` before calling `front()` or `pop()` to avoid runtime errors.
2. **Performance Misconception:** Using standard arrays without circular pointers leads to $O(n)$ time complexity due to shifting; always use built-in library queues or linked lists.

---

## 8. Related Patterns

Stack
↓
Queue
↓
Simple FIFO
↓
Breadth-First Search (BFS)
↓
Multi-Source BFS
↓
Level Order Traversal

---

## 9. Dry Run: `push(1), push(2), pop()`

| Step | Operation | Queue State | Front | Back |
| --- | --- | --- | --- | --- |
| **1** | push(1) | [1] | 1 | 1 |
| **2** | push(2) | [1, 2] | 1 | 2 |
| **3** | pop() | [2] | 2 | 2 |

---

## 10. Time and Space Complexity

| Operation | Time Complexity | Space Complexity |
| --- | --- | --- |
| Enqueue | $O(1)$ | $O(1)$ |
| Dequeue | $O(1)$ | $O(1)$ |
| Queue Storage | — | $O(n)$ |

### Important Observation

Elements are never shifted. Insertions happen at the back and removals happen at the front, allowing both operations to remain constant time.

---

## 11. How to Identify

* Must you process elements in arrival order?
* Does the oldest element need to be handled first?
* Is "first come, first served" critical?
* Are tasks processed sequentially?

---

## 12. Brute Force vs. Optimized

| Approach | Logic | Time | Space |
| --- | --- | --- | --- |
| **Array (shifting)** | Remove front by shifting all remaining elements. | $O(n)$ | $O(n)$ |
| **Queue** | Front and back operations only. | $O(1)$ | $O(n)$ |

---

##  Practice Problems

| Problem | Link |
| --- | --- |
| Implement Queue using Stacks | [https://leetcode.com/problems/implement-queue-using-stacks/](https://leetcode.com/problems/implement-queue-using-stacks/) |
| Design Circular Queue | [https://leetcode.com/problems/design-circular-queue/](https://leetcode.com/problems/design-circular-queue/) |

---
