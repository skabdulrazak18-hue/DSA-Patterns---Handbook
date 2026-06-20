
# Pattern Name: Dummy Node Technique

### Goal

Simplify Linked List operations (like insertion, deletion, or merging) by creating a "dummy" node at the head to handle edge cases where the head of the list might change.

---

## 1. Core Idea

When modifying a Linked List, the most annoying case is when you need to change the `head` (e.g., deleting the first node or merging two lists starting at the head). A "dummy" node acts as a temporary anchor, allowing you to treat the `head` like any other node in the list.

### Theory

By placing a placeholder node before the actual `head`, you ensure that the logic for node manipulation (`prev->next = ...`) always has a valid `prev` node to work with, even when operating at the very start of the list.

---

## 2. Why This Pattern Matters

1. **Eliminates Null Checks:** You don't have to write `if (head == NULL)` repeatedly.
2. **Standardizes Logic:** The code for the first node becomes identical to the code for all other nodes.
3. **Prevents Errors:** Significantly reduces "off-by-one" and null-pointer dereference bugs during head modifications.
4. **Simplifies Returns:** You can simply return `dummy.next` to get the actual head of the modified list.
5. **Makes Code Cleaner:** It makes Linked List code shorter and easier to debug.

---

## 3. When to Use

### Checklist

* You need to delete nodes from a list (especially the head).
* You are merging, splitting, or reordering linked lists.
* You are inserting nodes and the new node might become the new head.
* You are performing operations where the number of nodes in the list might change.

### Key Phrases to Watch For

* "Remove the n-th node from the end of the list..."
* "Merge two sorted lists..."
* "Remove all nodes with a certain value..."
* "Partition the list around a value..."

---

## 4. Typical Elements of the Pattern

### A. The Dummy Node

A `ListNode(0)` or `ListNode(-1)` created on the stack.

### B. The Tail Pointer

A pointer (often called `curr` or `tail`) that tracks the last processed node, which always starts at the `dummy` node.

### C. Return Value

Returning `dummy.next` after all operations are complete to bypass the placeholder.

---

## 5. Common Variations

| Variation | Strategy |
| --- | --- |
| **Deletion** | Skip nodes using `curr->next = curr->next->next` |
| **Insertion** | Insert new nodes after `curr` |
| **Merging** | Build a new list using a `tail` pointer |
| **Partitioning** | Use two dummy nodes (e.g., `less` and `greater`) |
| **Multiple Lists** | Maintain separate dummy heads for different groups |

---

## 6. Time and Space Complexity

| Operation | Time | Space |
| --- | --- | --- |
| Node Manipulation | $O(n)$ | $O(1)$ |

---

## 7. Common Pitfalls to Avoid

1. **Returning the Dummy:** Accidentally returning the dummy node instead of `dummy.next`.
2. **Updating Tails:** Forgetting to advance the `tail` pointer after attaching a new node.
3. **Memory Leaks:** Always prefer stack allocation (`ListNode dummy(0);`) to avoid memory management issues in C++.

---

## 8. Related Patterns

Linked List Traversal
↓
Dummy Node Technique
↓
Merge Lists / Partitioning

---

## 9. Template

```cpp
ListNode* processList(ListNode* head) {
    ListNode dummy(0); // Create dummy node on stack
    dummy.next = head;
    ListNode* curr = &dummy;

    while (curr->next != nullptr) {
        // Manipulate curr->next
        curr = curr->next;
    }
    return dummy.next; // Return the real head
}

```

---

## 10. How to Identify

* Is this a Linked List problem?
* Do you need to return a new head or potentially modify the existing head?
* Does the problem involve deleting or inserting multiple nodes?

---

## 11. Brute Force vs. Optimized

| Approach | Logic | Time | Space |
| --- | --- | --- | --- |
| **Brute Force** | Use extra conditional checks for the head node vs. others. | $O(n)$ | $O(1)$ |
| **Optimized** | Use a Dummy Node to unify the head case with all other cases. | $O(n)$ | $O(1)$ |

---

## 12. Example Problem: Remove Nth Node From End of List

Remove the $n^{th}$ node from the end of the list.

### Code

```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    ListNode dummy(0);
    dummy.next = head;
    ListNode *first = &dummy, *second = &dummy;
    
    // Gap of n
    for(int i = 0; i <= n; i++) first = first->next;
    
    // Move together
    while(first != nullptr) {
        first = first->next;
        second = second->next;
    }
    second->next = second->next->next;
    return dummy.next;
}

```

### Dry Run: List 1 → 2 → 3 → 4 → 5, n = 2

| Step | First Pointer | Second Pointer | List State | Action |
| --- | --- | --- | --- | --- |
| **Start** | Dummy | Dummy | 1 → 2 → 3 → 4 → 5 | Initialize |
| **1** | 3 | Dummy | 1 → 2 → 3 → 4 → 5 | Create gap |
| **2** | 5 | 2 | 1 → 2 → 3 → 4 → 5 | Move together |
| **3** | null | 3 | 1 → 2 → 3 → 4 → 5 | second reaches node before target |
| **4** | null | 3 | 1 → 2 → 3 → 5 | Delete node 4 |

### Complexity Analysis

* **Time:** $O(n)$ — The list is traversed at most twice.
* **Space:** $O(1)$ — Only one extra dummy node and a few pointers are used.

---

##  Practice Problems

### Easy

| Problem | Link |
| --- | --- |
| Remove Linked List Elements | [https://leetcode.com/problems/remove-linked-list-elements/](https://leetcode.com/problems/remove-linked-list-elements/) |
| Delete Node in a Linked List | [https://leetcode.com/problems/delete-node-in-a-linked-list/](https://leetcode.com/problems/delete-node-in-a-linked-list/) |

### Medium

| Problem | Link |
| --- | --- |
| Remove Nth Node From End of List | [https://leetcode.com/problems/remove-nth-node-from-end-of-list/](https://leetcode.com/problems/remove-nth-node-from-end-of-list/) |
| Partition List | [https://leetcode.com/problems/partition-list/](https://leetcode.com/problems/partition-list/) |
| Swap Nodes in Pairs | [https://leetcode.com/problems/swap-nodes-in-pairs/](https://leetcode.com/problems/swap-nodes-in-pairs/) |
| Odd Even Linked List | [https://leetcode.com/problems/odd-even-linked-list/](https://leetcode.com/problems/odd-even-linked-list/) |

### Hard

| Problem | Link |
| --- | --- |
| Merge k Sorted Lists | [https://leetcode.com/problems/merge-k-sorted-lists/](https://leetcode.com/problems/merge-k-sorted-lists/) |
| Reverse Nodes in k-Group | [https://leetcode.com/problems/reverse-nodes-in-k-group/](https://leetcode.com/problems/reverse-nodes-in-k-group/) |

---

