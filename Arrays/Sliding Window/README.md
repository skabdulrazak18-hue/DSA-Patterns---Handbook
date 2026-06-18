

# Sliding Window: Tracking Data in a Subset

### Goal

Create a "window" over a contiguous portion of data (like an array or string) to calculate results efficiently without recalculating overlapping elements.

---

# 1. Core Idea

The Sliding Window pattern is an optimization technique. Instead of recalculating a subset of data from scratch every time we move forward, we reuse the work we have already done.

When the window "slides" forward by one position, we simply:

1. **Remove** the effect of the element that falls out of the left side of the window.
2. **Add** the effect of the new element that enters the right side of the window.

> Keep track of the current state of a subset, and smoothly update it as the subset shifts.

---

# 2. Why This Pattern Matters

Sliding Window is a fundamental technique for array and string manipulation.

It helps:

1. Transform O(n * k) nested-loop solutions into O(n) linear solutions.
2. Avoid redundant computations (calculating the same overlapping items repeatedly).
3. Solve complex substring and subarray problems elegantly.

---

# 3. When to Use This Pattern

## The Checklist

Think about Sliding Window when:
✓ The problem involves arrays, strings, or linked lists.
✓ You are asked to find a **contiguous** subarray or substring.
✓ You need to calculate a sum, average, maximum, or minimum of a subset.
✓ You need to find the longest or shortest sequence that meets a certain condition.

---

## Key Phrases to Watch For

Problems containing phrases like:

* "Maximum sum of a contiguous subarray of size k..."
* "Longest substring without repeating characters..."
* "Smallest subarray with a sum greater than or equal to..."
* "Find all anagrams in a string..."

---

# 4. Theory Behind the Pattern

Imagine a window frame placed over a specific number of items in an array. To look at the next set of items, you don't take the frame off and rebuild it; you just slide it over by one space.

Because you only do a constant amount of work (usually one addition and one subtraction) each time the window slides, the overall time complexity drops from quadratic O(n²) or O(n*k) down to linear O(n).

---

# 5. Common Variations

---

## A. Fixed Window Size

The size of the window (`k`) stays exactly the same throughout the entire array.

### Visualization

Window size `k = 3`

```text
[2   1   5]  1   3   2   (Sum = 8)

```

Slide right:

```text
 2  [1   5   1]  3   2   (Sum = 8 - 2 + 1 = 7)

```

Slide right:

```text
 2   1  [5   1   3]  2   (Sum = 7 - 1 + 3 = 9)

```

### Template (C++)

```cpp
int maxSum = 0, windowSum = 0;

// 1. Calculate the first window
for(int i = 0; i < k; i++) {
    windowSum += arr[i];
}
maxSum = windowSum;

// 2. Slide the window
for(int i = k; i < arr.size(); i++) {
    windowSum += arr[i] - arr[i - k]; // Add new right, subtract old left
    maxSum = max(maxSum, windowSum);
}

```

---

## B. Variable Window Size

The window grows (expands to the right) and shrinks (contracts from the left) based on a specific condition.

### Visualization

Find the longest subarray with a sum ≤ 8.

```text
[2   1   5]  2   8   (Sum = 8. Valid! Length = 3)

```

Expand right:

```text
[2   1   5   2]  8   (Sum = 10. Invalid! Must shrink from left)

```

Shrink left:

```text
 2  [1   5   2]  8   (Sum = 8. Valid again! Length = 3)

```

### Template (C++)

```cpp
int left = 0, maxLength = 0, currentSum = 0;

for(int right = 0; right < arr.size(); right++) {
    currentSum += arr[right]; // Add the new element
    
    // While the condition is invalid, shrink the window from the left
    while(currentSum > target) {
        currentSum -= arr[left];
        left++;
    }
    
    // Update the best result
    maxLength = max(maxLength, right - left + 1);
}

```

---

# 6. From Brute Force to Optimized Solution

## Brute Force

### Idea

Recalculate the target condition (sum, max, min) for every single subarray from scratch using nested loops.

### Time Complexity

O(n * k)

### Skeleton

```cpp
for(int i = 0; i <= n - k; i++) 
{
    int currentSum = 0;
    for(int j = i; j < i + k; j++) 
    {
        currentSum += arr[j];
    }
}

```

---

## Optimized Approach

### Idea

Calculate the first window, then simply add the new element and subtract the old element as you slide forward.

### Time Complexity

O(n)

### Skeleton

```cpp
// 1. Get first window
// 2. Slide window: current += new_element - old_element

```

---

# 7. Complexity Analysis

| Complexity | Value | Reason |
| --- | --- | --- |
| **Time Complexity** | **O(n)** | Both the `left` and `right` pointers only move forward. Each element is processed at most twice. |
| **Space Complexity** | **O(1)** or **O(k)** | O(1) for sums/counts. O(k) if using a Hash Map to track frequencies inside the window. |

---

# 8. Common Pitfalls

## Off-by-One Errors

When calculating the length of a variable window, remember that the number of elements between index `left` and index `right` inclusive is `right - left + 1`. Forgetting the `+ 1` is a very common bug.

## Using a `while` instead of an `if` (or vice versa)

In variable windows, if you only need to find the *maximum* length, sometimes you can change the inner `while(invalid)` loop to an `if(invalid)`. This keeps the window at its maximum size as it slides, which can optimize the code slightly.

---

# 9. Related Patterns

```text
Two Pointers
     ↓
Sliding Window
     ↓
Dynamic Programming (Sometimes overlaps with String problems)

```

Sliding Window is essentially a specific variation of the **Same Direction Two Pointers** pattern. The two pointers act as the left and right boundaries of the window.

---

# 10. Pattern Recognition Flowchart

```text
Array, String, or Linked List?
        ↓
Looking for a CONTIGUOUS subarray or substring?
        ↓
Need to calculate sum, max, min, or length?
        ↓
Think Sliding Window

```

---

# 11. Example Problem

## Problem Information

| Problem | Difficulty | Pattern |
| --- | --- | --- |
| [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/) | Easy | Fixed Sliding Window |

---

### Intuition

Instead of calculating the sum of every possible block of `k` elements from scratch, we find the sum of the first `k` elements. Then, as we slide the window forward, we subtract the element that leaves the window and add the element that enters it.

---

### Code (C++)

```cpp
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        double currentSum = 0;
        
        // Build the first window
        for(int i = 0; i < k; i++) {
            currentSum += nums[i];
        }
        
        double maxSum = currentSum;
        
        // Slide the window
        for(int i = k; i < nums.size(); i++) {
            currentSum += nums[i] - nums[i - k];
            maxSum = max(maxSum, currentSum);
        }
        
        return maxSum / k;
    }
};

```

---

### Dry Run

Input: `nums = [1, 12, -5, -6, 50, 3]`, `k = 4`

| Step | Left Out | Right In | Window Array | Window Sum | Max Sum |
| --- | --- | --- | --- | --- | --- |
| Initial | - | - | `[1, 12, -5, -6]` | 2 | 2 |
| Slide 1 | Remove `1` | Add `50` | `[12, -5, -6, 50]` | 2 - 1 + 50 = 51 | 51 |
| Slide 2 | Remove `12` | Add `3` | `[-5, -6, 50, 3]` | 51 - 12 + 3 = 42 | 51 |

Output: `51 / 4 = 12.75`

---

# 12. Practice Problems

## Easy (Build Intuition)

| Problem | Link |
| --- | --- |
| Maximum Average Subarray I | [https://leetcode.com/problems/maximum-average-subarray-i/](https://leetcode.com/problems/maximum-average-subarray-i/) |
| Contains Duplicate II | [https://leetcode.com/problems/contains-duplicate-ii/](https://leetcode.com/problems/contains-duplicate-ii/) |

## Intermediate (Strengthen Pattern Recognition)

| Problem | Link |
| --- | --- |
| Longest Substring Without Repeating Characters | [https://leetcode.com/problems/longest-substring-without-repeating-characters/](https://leetcode.com/problems/longest-substring-without-repeating-characters/) |
| Max Consecutive Ones III | [https://leetcode.com/problems/max-consecutive-ones-iii/](https://leetcode.com/problems/max-consecutive-ones-iii/) |
| Fruit Into Baskets | [https://leetcode.com/problems/fruit-into-baskets/](https://leetcode.com/problems/fruit-into-baskets/) |

## Hard (Challenge Yourself)

| Problem | Link |
| --- | --- |
| Sliding Window Maximum | [https://leetcode.com/problems/sliding-window-maximum/](https://leetcode.com/problems/sliding-window-maximum/) |
| Minimum Window Substring | [https://leetcode.com/problems/minimum-window-substring/](https://leetcode.com/problems/minimum-window-substring/) |
