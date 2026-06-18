

# Pattern Name: Reversal

### Goal

Efficiently manipulate a sequence (array, string, or linked list) by reversing segments to achieve a transformation in $O(n)$ time and $O(1)$ space.

---

## 1. Core Idea

Reversal is the act of flipping the order of elements within a sequence. By strategically reversing specific segments, you can rotate arrays, reorder words, or pivot linked lists without needing auxiliary data structures.

### Theory

The reversal principle relies on the property that reversing a segment twice restores its original order. By performing a sequence of reversals (e.g., reversing the whole structure, then reversing individual segments), you can reorder data in-place.

---

## 2. Why This Pattern Matters

1. Solves rotation/shift problems in $O(1)$ space.
2. Converts $O(n)$ space solutions into $O(1)$ space.
3. Provides a fundamental technique for manipulating linked list pointers.
4. Essential for structural reordering tasks.

---

## 3. When to Use

### Checklist

* Need to rotate an array or string by $k$ positions.
* Need to reverse words in a sentence.
* Need to reorder a Linked List (e.g., reverse between indices).
* Need to perform in-place transformations with minimal memory.

### Key Phrases to Watch For

* "Rotate array..."
* "Reverse words..."
* "Reorder linked list..."
* "Flip sequence..."

---

## 4. Typical Elements of the Pattern

### A. The Helper/Logic

For arrays/strings, a `reverse(start, end)` helper using two pointers. For linked lists, the `prev`, `curr`, `next` pointer swap.

### B. The Sequence Strategy

A specific order of operations (e.g., Reverse all, then reverse specific parts).

### C. In-Place Swapping

Swapping elements until the pointers meet (Arrays) or pointers reach the end (Linked Lists).

---

## 5. Common Variations

| Variation | Strategy |
| --- | --- |
| **Reverse Entire Sequence** | Reverse all elements |
| **Array Rotation** | Reverse all $\to$ Reverse parts |
| **Reverse Words** | Reverse whole string $\to$ Reverse each word |
| **Linked List Reversal** | Reverse links using `prev`, `curr`, `next` pointers |
| **Reverse K-Group** | Reverse blocks of size `k` |

---

## 6. Time and Space Complexity

| Operation | Time | Space |
| --- | --- | --- |
| Full Reversal | $O(n)$ | $O(1)$ |
| Rotation/Reordering | $O(n)$ | $O(1)$ |

---

## 7. Common Pitfalls to Avoid

1. Forgetting to handle cases where $k > n$ (use $k = k \% n$).
2. Incorrectly defining the boundaries (`start` and `end` indices).
3. Losing the `next` pointer when reversing a Linked List.

---

## 8. Related Patterns

Two Pointers
↓
Reversal
↓
Rotation / Transformation

---

## 9. Templates

```cpp
// Array/String Reversal (Two Pointers)
void reverse(vector<int>& nums, int start, int end) {
    while (start < end) swap(nums[start++], nums[end--]);
}

// Linked List Reversal (Pointer Swap)
ListNode* reverseList(ListNode* head) {
    ListNode *prev = nullptr, *curr = head;
    while (curr) {
        ListNode* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}

```

---

## 10. How to Identify

* Is there a request to shift, rotate, or reorder elements?
* Is $O(1)$ auxiliary space complexity requested?
* Does the transformation involve "flipping" segments of the input?

---

## 11. Brute Force vs. Optimized

| Approach | Logic | Time | Space |
| --- | --- | --- | --- |
| **Brute Force** | Use an extra array/list, copy elements in new order. | $O(n)$ | $O(n)$ |
| **Optimized** | Use the Reversal strategy to flip elements in-place. | $O(n)$ | $O(1)$ |

---

## 12. Example Problem: Rotate Array

Rotate `[1,2,3,4,5]` to the right by $k=2$.

### Code

```cpp
void rotate(vector<int>& nums, int k) {
    int n = nums.size();
    k %= n;
    reverse(nums, 0, n - 1); // Reverse all
    reverse(nums, 0, k - 1); // Reverse first k
    reverse(nums, k, n - 1); // Reverse remaining
}

```

### Dry Run

| Step | Operation | Array State |
| --- | --- | --- |
| **Initial** | - | `[1, 2, 3, 4, 5]` |
| **1** | Reverse All | `[5, 4, 3, 2, 1]` |
| **2** | Reverse 0 to k-1 | `[4, 5, 3, 2, 1]` |
| **3** | Reverse k to end | `[4, 5, 1, 2, 3]` |

### Complexity

Complexity Analysis
Time Complexity: 
$O(n)$We perform three reversal operations. Each reversal touches a subset of the $n$ elements. Total operations remain proportional to $n$.
Space Complexity: 
$O(1)$We modify the array in-place. Only a few integer variables (start, end, k) are used for pointer management, which does not grow with the input size.
---

## 13. Practice Problems

### Easy

| Problem | Link |
| --- | --- |
| Reverse String | [Link](https://leetcode.com/problems/reverse-string/) |
| Reverse Linked List | [Link](https://leetcode.com/problems/reverse-linked-list/) |

### Intermediate

| Problem | Link |
| --- | --- |
| Rotate Array | [Link](https://leetcode.com/problems/rotate-array/) |
| Reverse Words in a String | [Link](https://leetcode.com/problems/reverse-words-in-a-string/) |
| Rotate String | [Link](https://leetcode.com/problems/rotate-string/) |

### Hard

| Problem | Link |
| --- | --- |
| Reverse Nodes in k-Group | [Link](https://leetcode.com/problems/reverse-nodes-in-k-group/) |

---
