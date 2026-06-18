

# Pattern Name: Fast & Slow Pointers

### Goal

Efficiently traverse a structure using two pointers moving at different speeds to detect cycles or locate positional elements without extra memory.

---

## 1. Core Idea

Initialize two pointers: `slow` (moves 1 step) and `fast` (moves 2 steps). If there is a cycle, the `fast` pointer will eventually "lap" the `slow` pointer inside the loop.

### Theory

The `fast` pointer covers distance twice as quickly as the `slow` pointer. In a cycle, the distance between them decreases by 1 unit every iteration, ensuring they must eventually collide.

---

## 2. Why This Pattern Matters

1. Solves cycle detection in $O(1)$ space.
2. Finds midpoints in a single pass ($O(n)$ time).
3. Eliminates the need for hash maps to track visited nodes.
4. Converts many $O(n)$ space solutions into $O(1)$ space solutions.

---

## 3. When to Use

### Checklist

* Data structure contains a potential cycle (Linked List/Array).
* Need to find the midpoint of a list without knowing its length.
* Need to find the $k^{th}$ element from the end of a list.

### Key Phrases to Watch For

* "Determine if a Linked List has a cycle..."
* "Find the middle of the Linked List..."
* "Find the start of the cycle..."
* "Find the duplicate number..."

---

## 4. Typical Elements of the Pattern

### A. Initialization

Two pointers `slow` and `fast` starting at the same node.

### B. Movement Logic

`slow = slow.next;` and `fast = fast.next.next;`

### C. Termination

Loop ends if `fast` or `fast.next` is `null` (no cycle) or `slow == fast` (cycle detected).

---

## 5. Common Variations

| Variation | Purpose |
| --- | --- |
| **Cycle Detection** | Determine whether a cycle exists |
| **Cycle Start Point** | Find where the cycle begins |
| **Find Middle Node** | Locate the midpoint in one traversal |
| **Find Duplicate Number** | Detect duplicates using cycle theory |
| **Find Cycle Length** | Compute the size of the cycle |

---

## 6. Time and Space Complexity

| Operation | Time | Space |
| --- | --- | --- |
| Cycle Detection | $O(n)$ | $O(1)$ |
| Find Midpoint | $O(n)$ | $O(1)$ |

---

## 7. Common Pitfalls to Avoid

1. Forgetting to check `fast->next` before `fast->next->next` (causes Segfault).
2. Not moving the `fast` pointer twice as fast, which prevents convergence.
3. Incorrectly handling even vs. odd length list midpoints.

---

## 8. Related Patterns

Two Pointers
↓
Fast & Slow Pointers
↓
Cycle Detection

---

## 9. Template

```cpp
// Cycle Detection
ListNode *slow = head, *fast = head;
while (fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;
    if (slow == fast) return true; // Cycle detected
}
return false;

// Midpoint Finder
ListNode *slow = head, *fast = head;
while (fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;
}
return slow; // slow is now at middle

```

---

## 10. How to Identify

* Does the problem involve traversing nodes without extra storage?
* Is there a hint that the structure might loop back on itself?
* Does the problem require finding a "middle" or "nth position" in one pass?

---

## 11. Brute Force vs. Optimized

| Approach | Logic | Time | Space |
| --- | --- | --- | --- |
| **Brute Force** | Use a `HashSet` to store every visited node. If you see a node again, it's a cycle. | $O(n)$ | $O(n)$ |
| **Optimized** | Use `Fast & Slow` pointers to traverse. No extra space needed. | $O(n)$ | $O(1)$ |

---

## 12. Example Problem: Linked List Cycle

Determine if a linked list has a cycle.

### Code

```cpp
bool hasCycle(ListNode *head) {
    ListNode *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}

```

### Dry Run

| Step | Slow Node | Fast Node | Notes |
| --- | --- | --- | --- |
| 1 | 1 | 1 | Start at head |
| 2 | 2 | 3 | Move pointers |
| 3 | 3 | 5 | Move pointers |
| 4 | 4 | 4 | Collision Detected (Cycle found) |

### Time and Space Complexity

* **Time Complexity:** $O(n)$, as pointers traverse the structure at most twice.
* **Space Complexity:** $O(1)$, using only two pointer variables regardless of list size.

---

## Practice Problems

### Easy

1. Linked List Cycle — [Link](https://leetcode.com/problems/linked-list-cycle/)
2. Middle of the Linked List — [Link](https://leetcode.com/problems/middle-of-the-linked-list/)

### Intermediate

1. Linked List Cycle II — [Link](https://leetcode.com/problems/linked-list-cycle-ii/)
2. Find the Duplicate Number — [Link](https://leetcode.com/problems/find-the-duplicate-number/)
3. Happy Number — [Link](https://leetcode.com/problems/happy-number/)

---

