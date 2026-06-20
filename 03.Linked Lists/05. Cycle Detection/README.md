

# Pattern Name: Cycle Detection (Floyd's Algorithm)

### Goal

Determine if a linked list contains a cycle (an infinite loop) and, if it does, identify the starting node of that cycle using constant memory.

---

## 1. Core Idea

This is a specialized application of the **Fast & Slow Pointers** pattern. By using two pointers moving at different speeds, if there is a cycle, the faster pointer will eventually enter the loop and "lap" the slower pointer, confirming the cycle's existence.

### Theory

If a cycle exists, the distance between the `slow` and `fast` pointers reduces by 1 in each step. Once they meet, we can mathematically locate the start of the cycle by resetting one pointer to the `head` and moving both at the same speed; the point where they meet again is the entry point of the loop.

---

## 2. Why This Pattern Matters

1. **Space Efficiency:** Detects cycles in $O(1)$ space, avoiding the need for $O(n)$ hash maps.
2. **Infinite Loop Prevention:** Critical for systems that traverse references or pointers.
3. **Foundation for Graph Theory:** This logic is a precursor to understanding cycle detection in general graphs.
4. **Interview Essential:** A classic "trick" question that tests deep pointer manipulation skills.

---

## 3. When to Use

### Checklist

* You are traversing nodes where one node might point to a previous node.
* The problem involves detecting infinite loops.
* Memory constraints are strict (e.g., $O(1)$ space).
* You need to find the specific node where a cycle begins.

### Key Phrases to Watch For

* "Determine if a Linked List has a cycle..."
* "Find the node where the cycle begins..."
* "Detect an infinite loop..."

---

## 4. Typical Elements of the Pattern

### A. The Two-Pointer Setup

A `slow` pointer (1 step) and a `fast` pointer (2 steps).

### B. The Meeting Point

The stage where `slow == fast` is true, confirming that a cycle exists.

### C. The Reset Phase

Resetting `slow` to the `head` while keeping `fast` at the meeting point to find the cycle start.

---

## 5. Common Variations

| Variation | Strategy |
| --- | --- |
| **Detection Only** | Return true if pointers meet, false if `fast` hits `null`. |
| **Find Cycle Start** | Reset `slow` to head; move both 1 step until they meet. |
| **Find Cycle Length** | Keep `slow` stationary and move `fast` until they meet again, counting steps. |

---

## 6. Time and Space Complexity

| Operation | Time | Space |
| --- | --- | --- |
| Cycle Detection | $O(n)$ | $O(1)$ |

---

## 7. Common Pitfalls to Avoid

1. **Null Pointer Errors:** Always ensure `fast` and `fast->next` are not null before accessing them.
2. **Speed Mismatch:** Forgetting to move the fast pointer at exactly double the speed of the slow one.
3. **Initialization:** Starting `slow` and `fast` at different positions instead of at the `head`.

---

## 8. Related Patterns

Linked List Traversal
↓
Fast & Slow Pointers
↓
Cycle Detection

---

## 9. Template

```cpp
ListNode* detectCycle(ListNode* head) {
    ListNode *slow = head, *fast = head;
    
    // 1. Detect if cycle exists
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) {
            // 2. Find start of cycle
            slow = head;
            while (slow != fast) {
                slow = slow->next;
                fast = fast->next;
            }
            return slow;
        }
    }
    return nullptr;
}

```

---

## 10. How to Identify

* Are you traversing a linked list or linked structure?
* Is there a possibility that a node's `next` pointer refers to a previous node?
* Is $O(1)$ space required to identify an infinite loop?

---

## 11. Brute Force vs. Optimized

| Approach | Logic | Time | Space |
| --- | --- | --- | --- |
| **Brute Force** | Store all visited nodes in a Hash Set. If you encounter a node already in the set, a cycle exists. | $O(n)$ | $O(n)$ |
| **Optimized** | Use Floyd’s Cycle-Finding Algorithm (Fast & Slow Pointers). | $O(n)$ | $O(1)$ |

---

## 12. Example Problem: Linked List Cycle II

Return the node where the cycle begins.

### Code

```cpp
ListNode* detectCycle(ListNode* head) {
    ListNode *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) {
            slow = head;
            while (slow != fast) {
                slow = slow->next;
                fast = fast->next;
            }
            return slow;
        }
    }
    return nullptr;
}

```

### Dry Run: 3 → 2 → 0 → -4 (Cycle starts at 2)

| Step | Slow | Fast | List State | Action |
| --- | --- | --- | --- | --- |
| **Start** | 3 | 3 | 3→2→0→-4→(2) | Initialize |
| **1** | 2 | 0 | 3→2→0→-4→(2) | Move pointers |
| **2** | 0 | -4 | 3→2→0→-4→(2) | Continue |
| **3** | -4 | -4 | 3→2→0→-4→(2) | Collision detected |
| **4** | 3 | -4 | 3→2→0→-4→(2) | Reset slow to head |
| **5** | 2 | 2 | 3→2→0→-4→(2) | Cycle start found |

### Time and Space Complexity

* **Time Complexity:** $O(n)$ — In the worst case, both pointers traverse the list at most twice.
* **Space Complexity:** $O(1)$ — Only two pointers are used regardless of the size of the list.

---

##  Practice Problems

### Easy

| Problem | Link |
| --- | --- |
| Linked List Cycle | [https://leetcode.com/problems/linked-list-cycle/](https://leetcode.com/problems/linked-list-cycle/) |

### Medium

| Problem | Link |
| --- | --- |
| Linked List Cycle II | [https://leetcode.com/problems/linked-list-cycle-ii/](https://leetcode.com/problems/linked-list-cycle-ii/) |
| Happy Number | [https://leetcode.com/problems/happy-number/](https://leetcode.com/problems/happy-number/) |
| Find the Duplicate Number | [https://leetcode.com/problems/find-the-duplicate-number/](https://leetcode.com/problems/find-the-duplicate-number/) |

### Hard

| Problem | Link |
| --- | --- |
| Circular Array Loop | [https://leetcode.com/problems/circular-array-loop/](https://leetcode.com/problems/circular-array-loop/) |

---

