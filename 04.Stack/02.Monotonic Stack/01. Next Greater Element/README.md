

# Pattern Name: Next Greater Element (NGE)

### Goal

Find the first element to the right of each position that is strictly greater than the current element, efficiently and in linear time.

---

## 1. Core Idea

For every element in an array, we need to find the nearest element to its right that is larger. Using a brute-force nested loop would compare every pair, but a **Monotonic Stack** allows us to maintain a "stack of potential candidates" and solve this in a single pass.

### Theory

We iterate through the array, maintaining a stack of elements that are waiting to find their "Next Greater" partner. When we encounter a number larger than the top of our stack, we have found the answer for that top element.

---

## 2. Why Next Greater Element Matters

1. **Linear Efficiency:** Reduces complexity from $O(n^2)$ to $O(n)$.
2. **Foundational Pattern:** It is the primary application of the Monotonic Stack, serving as the basis for more complex range-based problems.
3. **Information Discovery:** Essential for processing streams or sequences where the "future" impacts the "present."

---

## 3. When to Use

### Checklist

* You need to find the "next" element that satisfies a condition (greater/smaller).
* You are looking for the "nearest" element in a sequence.
* The brute-force solution involves nested loops scanning to the right.

### Key Phrases to Watch For

* "Next greater element..."
* "Find the first element to the right that is larger..."
* "Daily temperature increase..."

---

## 4. Typical Elements of the Pattern

### A. The Monotonic Stack

Stores indices or values in **decreasing order**. Any element smaller than the current one is popped because its "next greater" has been found.

### B. The Result Array

Initialized to -1 (default), it is updated whenever we pop an element from the stack.

### C. The Traversal

A single left-to-right pass.

---

## 5. Common Variations

| Variation | Strategy |
| --- | --- |
| **Next Smaller** | Keep stack in increasing order; pop when `current < top`. |
| **Previous Greater** | Traverse left-to-right, keep stack decreasing (finds the first larger to the left). |
| **Circular Array** | Use modulo (`i % n`) and iterate twice to handle wrap-around. |

---

## 6. Applications

### Direct Applications

* Next Greater Element I
* Next Greater Element II (Circular)
* Daily Temperatures
* Stock Span

### Foundation For

* Largest Rectangle in Histogram
* Trapping Rain Water
* Sum of Subarray Minimums

---

## 7. Template (Decreasing Stack)

```cpp
vector<int> nextGreaterElements(vector<int>& nums) {
    int n = nums.size();
    vector<int> res(n, -1);
    stack<int> st; // Store indices
    for (int i = 0; i < n; i++) {
        while (!st.empty() && nums[i] > nums[st.top()]) {
            res[st.top()] = nums[i];
            st.pop();
        }
        st.push(i);
    }
    return res;
}

```

---

## 8. Common Pitfalls to Avoid

1. **Pushing/Popping Logic:** Accidentally reversing the stack monotonicity (e.g., using an increasing stack for a "greater" problem).
2. **Handling Remainders:** Forgetting that elements remaining in the stack at the end do not have a "next greater" element.
3. **Index vs. Value:** Deciding whether to store the index or the value in the stack (indices are generally more versatile).

---

## 9. Related Patterns

Stack → Monotonic Stack → Next Greater Element
Stack → Monotonic Stack → Largest Rectangle in Histogram

---

## 10. Example Problem: Next Greater Element

Given an array, return the next greater element for every item.

### Code

```cpp
vector<int> nextGreater(vector<int>& nums) {
    stack<int> s;
    vector<int> res(nums.size(), -1);
    for(int i = 0; i < nums.size(); i++) {
        while(!s.empty() && nums[i] > nums[s.top()]) {
            res[s.top()] = nums[i];
            s.pop();
        }
        s.push(i);
    }
    return res;
}

```

---

## 11. Dry Run: `nums = [2, 1, 3]`

| Step | Action | Stack | Result Array |
| --- | --- | --- | --- |
| **1** | Process 2 | [0] | [-1, -1, -1] |
| **2** | Process 1 | [0, 1] | [-1, -1, -1] |
| **3** | Process 3 | [2] | [3, 3, -1] |

---

## 12. Time and Space Complexity

| Operation | Time Complexity | Space Complexity |
| --- | --- | --- |
| Traversal | $O(n)$ | $O(n)$ |

### Important Observation

Every element is pushed onto the stack exactly once and popped at most once, ensuring $O(n)$ time complexity regardless of the nesting or structure.

---

## 13. How to Identify

* Do you need to compare an element with future elements to its right?
* Can you maintain a decreasing order in the stack to easily identify when a "greater" element arrives?

---

## 14. Brute Force vs. Optimized

| Approach | Logic | Time | Space |
| --- | --- | --- | --- |
| **Brute Force** | Nested loops check every element to the right. | $O(n^2)$ | $O(1)$ |
| **Optimized** | Single pass with monotonic stack. | $O(n)$ | $O(n)$ |

---

## 15. Practice Problems

| Difficulty | Problem | Link |
| --- | --- | --- |
| **Easy** | Next Greater Element I | [https://leetcode.com/problems/next-greater-element-i/](https://leetcode.com/problems/next-greater-element-i/) |
| **Medium** | Daily Temperatures | [https://leetcode.com/problems/daily-temperatures/](https://leetcode.com/problems/daily-temperatures/) |
| **Medium** | Next Greater Element II | [https://leetcode.com/problems/next-greater-element-ii/](https://leetcode.com/problems/next-greater-element-ii/) |

---

