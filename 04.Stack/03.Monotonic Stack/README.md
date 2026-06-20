

# Pattern Name: Monotonic Stack

### Goal

Efficiently maintain ordered relationships between elements to answer nearest greater/smaller and range-based queries in linear time.

---

## 1. Core Idea

A **Monotonic Stack** is a stack where elements are kept in a strictly increasing or decreasing order. As you iterate through an array, you "pop" elements that violate this order until the property is restored, then push the current element.

### Theory

Whenever a new element breaks the monotonic property, elements are removed. Because these elements are "resolved" by the current element, they can be permanently discarded, ensuring each element is pushed and popped at most once.

---

## 2. Why Monotonic Stack Matters

1. **Efficiency:** Optimizes $O(n^2)$ "next/previous" searches to $O(n)$.
2. **Contextual Awareness:** Allows each element to efficiently "resolve" the state of its neighbors.
3. **Versatility:** Serves as the backbone for complex problems like histograms and rain-water trapping.
4. **Boundary Discovery:** Efficiently determines left and right boundaries for range-based problems.

---

## 3. When to Use

### Checklist

* You need the "first" element to the left/right that is greater/smaller.
* Brute-force approach uses $O(n^2)$ nested loops.
* Each element becomes "useless" once a larger/smaller neighbor is encountered.
* You need to track local extrema (peaks/valleys) across ranges.

### Key Phrases to Watch For

* "Next/Previous greater/smaller element..."
* "Daily temperatures..."
* "Largest rectangle..."
* "Trapping rain water..."

---

## 4. Typical Elements of the Pattern

### A. The Monotonic Property

The stack maintains either a strictly increasing or decreasing order.

### B. The Popping Phase

When the current element violates the stack property, pop elements. This is the moment the current element **resolves** the stack top.

### C. The Pushing Phase

Push the current element to maintain order for future comparisons.

---

## 5. Common Variations

| Stack Type | Maintains | Common Queries |
| --- | --- | --- |
| **Increasing Stack** | Increasing order | Previous Smaller, Next Smaller |
| **Decreasing Stack** | Decreasing order | Previous Greater, Next Greater |

*Note: Traversal direction (left→right or right→left) determines whether we obtain previous or next relationships.*

---

## 6. Applications

### Neighbor Queries

* Next Greater Element
* Previous Greater Element
* Next Smaller Element
* Previous Smaller Element
* Daily Temperatures
* Stock Span

### Range Problems

* Largest Rectangle in Histogram
* Trapping Rain Water
* Sum of Subarray Minimums

---

## 7. Time and Space Complexity

| Operation | Time | Space |
| --- | --- | --- |
| Monotonic Stack | $O(n)$ | $O(n)$ |

### Important Observations

* **Efficiency:** Each element is pushed once and popped once. Even though a `while` loop appears inside a `for` loop, the total complexity is $O(n)$, not $O(n^2)$.
* **Philosophy:** Elements that can no longer contribute to future answers are removed permanently. Once an element is popped, it never returns to the stack.

---

## 8. Common Pitfalls to Avoid

1. **Not Popping Enough:** Forgetting to pop *all* violating elements, not just the top one.
2. **Empty Stack:** Forgetting to handle the case where the stack is empty after popping.
3. **Indices vs. Values:** Prefer storing **indices** in the stack; they provide more context (distance, array bounds) than values alone.
4. **Using `>` vs `>=` Incorrectly:** Different problems require different handling of equal elements. Choosing the wrong comparison can produce incorrect boundaries.

---

## 9. Related Patterns

Stack
↓
Monotonic Stack
↓
Neighbor Queries
↓
Range Problems

---

## 10. Template

```cpp
for(int i = 0; i < n; i++) {

    // Current element resolves previous elements
    while(!st.empty() && violatesMonotonicProperty(nums[i], nums[st.top()])) {

        // Process the resolved element
        st.pop();
    }

    // Current element becomes a candidate
    st.push(i);
}

```

---

## 11. How to Identify

* Are you searching for the nearest greater/smaller element?
* Does a brute-force nested loop suggest $O(n^2)$ complexity?
* Can elements be discarded permanently once a "dominating" neighbor is found?

---

## 12. Brute Force vs. Optimized

| Approach | Logic | Time | Space |
| --- | --- | --- | --- |
| **Brute Force** | Nested loops checking every element in range. | $O(n^2)$ | $O(1)$ |
| **Optimized** | Monotonic stack tracks potential candidates in one pass. | $O(n)$ | $O(n)$ |

---

## 13. Example Problem: Next Greater Element I

Find the next greater element for each item in an array.

### Code

```cpp
vector<int> nextGreaterElement(vector<int>& nums) {
    stack<int> st;
    vector<int> res(nums.size(), -1);
    for (int i = 0; i < nums.size(); i++) {
        // Decreasing stack resolves next greater elements
        while (!st.empty() && nums[i] > nums[st.top()]) {
            res[st.top()] = nums[i]; 
            st.pop();
        }
        st.push(i);
    }
    return res;
}

```

### Dry Run: `nums = [2, 1, 3]`

| Step | Current | Stack (Indices) | Action | Result |
| --- | --- | --- | --- | --- |
| **1** | 2 | [0] | Push 0 | [-1, -1, -1] |
| **2** | 1 | [0, 1] | Push 1 | [-1, -1, -1] |
| **3** | 3 | [2] | Pop 1, Pop 0 | [3, 3, -1] |

---

## 14. Practice Problems

### Easy

| Problem | Link |
| --- | --- |
| Next Greater Element I | [https://leetcode.com/problems/next-greater-element-i/](https://leetcode.com/problems/next-greater-element-i/) |

### Medium

| Problem | Link |
| --- | --- |
| Daily Temperatures | [https://leetcode.com/problems/daily-temperatures/](https://leetcode.com/problems/daily-temperatures/) |
| Online Stock Span | [https://leetcode.com/problems/online-stock-span/](https://leetcode.com/problems/online-stock-span/) |

### Hard

| Problem | Link |
| --- | --- |
| Largest Rectangle in Histogram | [https://leetcode.com/problems/largest-rectangle-in-histogram/](https://leetcode.com/problems/largest-rectangle-in-histogram/) |
| Trapping Rain Water | [https://leetcode.com/problems/trapping-rain-water/](https://leetcode.com/problems/trapping-rain-water/) |

---

