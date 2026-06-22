
# Pattern Name: Min Stack

### Goal

Retrieve the minimum element from a stack in constant time $O(1)$, in addition to standard stack operations (`push`, `pop`, `top`).

---

## 1. Core Idea

A **Min Stack** maintains two pieces of information for every element pushed onto the stack: the value itself and the minimum value encountered up to that point. By storing the minimum at every level, we can instantly access it regardless of how many elements are in the stack.

### Theory

We use an auxiliary structure to track the history of minimums. When we `push`, we compare the new value with the current minimum. When we `pop`, the previous minimum is automatically restored.

---

## 2. Why Min Stack Matters

1. **Constant-Time Queries:** Essential for algorithms that require frequent "minimum" lookups without scanning the stack.
2. **State Management:** Demonstrates how to trade $O(n)$ space for $O(1)$ time in performance-critical applications.
3. **Design Pattern:** Serves as a fundamental example of how to augment basic data structures with extra state.

---

## 3. When to Use

### Checklist

* You need standard stack operations (`push`, `pop`, `top`).
* You need the minimum element of the stack at any given time.
* Both operations must be $O(1)$ time complexity.

### Key Phrases to Watch For

* "Design a stack that supports getMin..."
* "Find the minimum in constant time..."

---

## 4. Typical Elements of the Pattern

### A. The Primary Stack

Stores the actual data values being pushed by the user.

### B. The Auxiliary Tracker

A secondary structure that keeps track of the "minimum seen so far" corresponding to every element in the primary stack.

### C. The Min Update Rule

When pushing, `newMin = min(currentVal, previousMin)`.

---

## 5. Common Variations

| Variation | Strategy |
| --- | --- |
| **Secondary Stack** | Push to `minStack` only if `val <= minStack.top()`. |
| **Pair-based Stack** | Store `(val, min)` tuples in a single stack. |
| **Encoded Values** | Store transformed values to save space (e.g., `2 * val - min`). |

---

## 6. Applications

* Sliding Window Minimum
* Largest Rectangle in Histogram
* Online Minimum Queries
* Designing Data Structures with Extra State

---

## 7. Template (Secondary Stack)

```cpp
class MinStack {
    stack<int> s, minS;
public:
    void push(int val) {
        s.push(val);
        if (minS.empty() || val <= minS.top()) minS.push(val);
    }
    void pop() {
        if (s.top() == minS.top()) minS.pop();
        s.pop();
    }
    int getMin() { return minS.top(); }
};

```

---

## 8. Common Pitfalls to Avoid

1. **Updating Global Min:** Forgetting that `min` must change as elements are popped.
2. **Duplicate Minimums:** Failing to handle cases where the same minimum value is pushed multiple times (use `<=` or `>=` correctly).
3. **Empty Stack:** Always ensure the stack is not empty before performing operations.

---

## 9. Related Patterns

Stack → Min Stack → Specialized Stack Design

---

## 10. Example Problem: Min Stack

Design a stack that supports `push`, `pop`, `top`, and `getMin` in constant time.

### Code

```cpp
class MinStack {
    stack<int> s, minS;
public:
    void push(int val) {
        s.push(val);
        if (minS.empty() || val <= minS.top()) minS.push(val);
    }
    void pop() {
        if (s.top() == minS.top()) minS.pop();
        s.pop();
    }
    int top() { return s.top(); }
    int getMin() { return minS.top(); }
};

```

---

## 11. Dry Run: `push(5), push(3), push(7), pop()`

| Step | Action | Primary Stack | Min Stack |
| --- | --- | --- | --- |
| **1** | push(5) | [5] | [5] |
| **2** | push(3) | [5, 3] | [5, 3] |
| **3** | push(7) | [5, 3, 7] | [5, 3] |
| **4** | pop() | [5, 3] | [5, 3] |

---

## 12. Time and Space Complexity

| Operation | Time Complexity | Space Complexity |
| --- | --- | --- |
| Push/Pop/GetMin | $O(1)$ | $O(1)$ |
| Stack Storage | — | $O(n)$ |

### Important Observation

The stack remembers the minimum associated with every level. When an element is removed, the previous minimum is automatically restored, eliminating the need to scan the stack.

---

## 13. How to Identify

* Is the problem asking for a stack with an additional "get minimum" feature?
* Is $O(1)$ performance required for minimum retrieval?

---

## 14. Brute Force vs. Optimized

| Approach | Logic | Time | Space |
| --- | --- | --- | --- |
| **Brute Force** | Scan stack to find minimum upon request. | $O(n)$ | $O(1)$ |
| **Optimized** | Auxiliary structure tracks minimums in $O(1)$. | $O(1)$ | $O(n)$ |

---

## 15. Practice Problems

| Difficulty | Problem | Link |
| --- | --- | --- |
| **Easy** | Min Stack | [https://leetcode.com/problems/min-stack/](https://leetcode.com/problems/min-stack/) |
| **Medium** | Max Stack | [https://leetcode.com/problems/max-stack/](https://leetcode.com/problems/max-stack/) |
| **Medium** | Frequency Stack | [https://leetcode.com/problems/implement-stack-using-queues/](https://leetcode.com/problems/implement-stack-using-queues/) |

---
