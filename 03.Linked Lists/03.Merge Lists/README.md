

# Pattern Name: Merge Lists

### Goal

Efficiently combine two or more sorted arrays or linked lists into a single sorted structure while preserving order and minimizing extra memory.

---

## 1. Core Idea

Since the input lists are already sorted, we do not need to perform a full sort. We compare the "heads" of the lists, select the smaller value, and advance that pointer. We repeat this until all elements are merged.

### Theory

By comparing the front elements, we build the resulting list step-by-step. Using a **Dummy Node** to anchor the new list allows us to manage the head pointer effortlessly, ensuring we process each element exactly once.

---

## 2. Why This Pattern Matters

1. Solves the fundamental "Merge" step of Merge Sort in $O(N+M)$ time.
2. Demonstrates mastery of pointer manipulation.
3. Provides a clean, iterative approach to combining data.
4. Acts as the foundation for handling "K-Sorted" lists.
5. Frequently appears in Linked List, Heap, and Merge Sort problems.

---

## 3. When to Use

### Checklist

* Input lists or arrays are already sorted.
* Need to combine data from multiple sources while preserving sorted order.
* Memory constraints require an in-place or constant-space merge.

### Key Phrases to Watch For

* "Merge two sorted lists..."
* "Combine sorted arrays..."
* "Intersection of two linked lists..."
* "Sort a list that consists of two pre-sorted halves..."

---

## 4. Typical Elements of the Pattern

### A. The Dummy Node

A temporary node used to easily build the new list without manual head-pointer logic.

### B. Pointer Comparison

Compare the values at `list1->val` and `list2->val` at every iteration.

### C. The Tail Tracker

A pointer (often called `tail`) that tracks the last node added to the merged list.

---

## 5. Common Variations

| Variation | Strategy |
| --- | --- |
| **Simple Merge** | Two sorted lists into one new list. |
| **In-place Merge** | Reuse existing nodes to merge lists. |
| **Merge Arrays** | Two pointers on arrays (often starting from the end). |
| **K-Way Merge** | Merge $K$ sorted lists (requires a Min-Heap). |
| **Merge Sort** | Divide and Merge process. |

---

## 6. Time and Space Complexity

| Operation | Time | Space |
| --- | --- | --- |
| Merge Two Lists | $O(N + M)$ | $O(1)$ (in-place) |
| Merge K Lists | $O(N \log K)$ | $O(K)$ |

---

## 7. Common Pitfalls to Avoid

1. **Forgetting the Remainder:** Failing to attach the remaining nodes if one list is longer than the other.
2. **Memory Leaks:** Allocating new nodes when the problem asks for an in-place merge.
3. **Lost References:** Incorrectly updating the `next` pointer, causing parts of the original list to be orphaned.

---

## 8. Related Patterns

Two Pointers
↓
Merge Lists
↓
Merge Sort / Heaps

---

## 9. Template

```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode dummy(0);
    ListNode* tail = &dummy;
    
    while (l1 && l2) {
        if (l1->val < l2->val) {
            tail->next = l1;
            l1 = l1->next;
        }
        else {
            tail->next = l2;
            l2 = l2->next;
        }
        tail = tail->next;
    }
    
    tail->next = l1 ? l1 : l2; // Attach remaining
    return dummy.next;
}

```

---

## 10. How to Identify

* Are you dealing with two or more sorted structures?
* Is the goal to produce a single, combined sorted sequence?
* Can the next element be determined solely by looking at the current heads?

---

## 11. Brute Force vs. Optimized

| Approach | Logic | Time | Space |
| --- | --- | --- | --- |
| **Brute Force** | Concatenate all, then sort the result. | $O(N \log N)$ | $O(N)$ |
| **Optimized** | Use pointers to compare heads and merge in-place in one pass. | $O(N)$ | $O(1)$ |

---

## 12. Example Problem: Merge Two Sorted Lists

Combine `[1, 2, 4]` and `[1, 3, 4]`.

### Code

```cpp
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode dummy(0);
    ListNode* tail = &dummy;
    while (l1 && l2) {
        if (l1->val < l2->val) { tail->next = l1; l1 = l1->next; }
        else { tail->next = l2; l2 = l2->next; }
        tail = tail->next;
    }
    tail->next = l1 ? l1 : l2;
    return dummy.next;
}

```

### Dry Run

| Step | l1 Head | l2 Head | Node Added | Result |
| --- | --- | --- | --- | --- |
| **Start** | 1 | 1 | Dummy | Empty |
| **1** | 2 | 1 | 1 (l1) | 1 |
| **2** | 2 | 3 | 1 (l2) | 1→1 |
| **3** | 4 | 3 | 2 | 1→1→2 |
| **4** | 4 | 4 | 3 | 1→1→2→3 |
| **5** | NULL | 4 | 4 | 1→1→2→3→4 |
| **End** | NULL | NULL | Remaining 4 | 1→1→2→3→4→4 |

### Complexity Analysis

* **Time:** $O(N+M)$ where $N$ and $M$ are lengths of the lists. Each node is processed once.
* **Space:** $O(1)$ as we modify pointers in-place.

---

## 13. Practice Problems

### Easy

| Problem | Link |
| --- | --- |
| Merge Two Sorted Lists | [Link](https://leetcode.com/problems/merge-two-sorted-lists/) |

### Medium

| Problem | Link |
| --- | --- |
| Merge Sorted Array | [Link](https://leetcode.com/problems/merge-sorted-array/) |
| Sort List | [Link](https://leetcode.com/problems/sort-list/) |
| Merge Intervals | [Link](https://leetcode.com/problems/merge-intervals/) |

### Hard

| Problem | Link |
| --- | --- |
| Merge k Sorted Lists | [Link](https://leetcode.com/problems/merge-k-sorted-lists/) |

---
