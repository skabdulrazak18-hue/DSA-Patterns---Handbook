

# Kadane’s Algorithm: Maximum Subarray Sum

### Goal

Find the contiguous subarray within a one-dimensional array of numbers that has the largest sum, completing the task in optimal linear O(n) time.

---

# 1. Core Idea

Kadane's algorithm is an optimization technique that keeps a running total of the array elements.

**The Golden Rule:** A subarray with a negative sum will only decrease the value of whatever number comes next. Therefore, if your running sum ever drops below zero, it is completely useless. You drop it, reset the running sum to `0`, and start fresh from the next element.

> Keep adding numbers, but the moment your running total goes negative, throw it away and start over.

---

# 2. Why This Pattern Matters

Kadane's Algorithm is a legendary algorithm in computer science.

It helps:

1. Completely eliminate O(n²) or O(n³) brute-force nested loops.
2. Solve the problem in exactly one pass O(n).
3. Use absolutely zero extra memory O(1).
4. Build the foundation for understanding dynamic programming (DP).

---

# 3. When to Use This Pattern

## The Checklist

Think about Kadane's Algorithm when:

1. The problem involves a one-dimensional array.
2. You are asked to find a **contiguous** subarray.
3. You need to find the **maximum sum**.
4. The array contains both positive and **negative** numbers.

---

## Key Phrases to Watch For

Problems containing phrases like:

* "Maximum subarray sum..."
* "Largest contiguous sum..."
* "Maximum profit/score over a continuous period..."

---

# 4. Theory Behind the Pattern

The strength of Kadane's Algorithm comes from greedy decision-making.

Instead of looking at every possible starting and ending point, it makes a simple local decision at every single step: *Is the number I am standing on better off by itself, or is it better off being added to the running total from the previous numbers?* Because a negative prefix sum will always drag down a future sequence, instantly resetting to 0 ensures you are always building on the best possible foundation.

---

# 5. Common Variations

---

## A. Standard Kadane's (Find the Max Sum)

Used when the problem only asks for the final maximum integer value.

### Visualization

```text
Array: [-2,  1, -3,  4, -1,  2]

Sum:   -2 -> Reset to 0
Sum:    1 -> Keep (Max is 1)
Sum:   -2 -> Reset to 0
Sum:    4 -> Keep (Max is 4)
Sum:    3 -> Keep (Max is 4)
Sum:    5 -> Keep (Max is 5)

```

### Template

```cpp
int maxSubArray(vector<int>& nums) {
    int maxSum = nums[0]; 
    int currentSum = 0;

    for(int num : nums) {
        currentSum += num;
        maxSum = max(maxSum, currentSum); 

        if(currentSum < 0) {
            currentSum = 0; 
        }
    }
    
    return maxSum;
}

```

---

## B. Kadane's with Indices (Find the Actual Subarray)

Used when asked to return the start and end indices of the maximum subarray, not just the sum.

### Template

```cpp
vector<int> maxSubArrayIndices(vector<int>& nums) {
    int maxSum = nums[0], currentSum = 0;
    int maxStart = 0, maxEnd = 0, tempStart = 0;

    for(int i = 0; i < nums.size(); i++) {
        if(currentSum == 0) tempStart = i; // Start of a new potential max subarray
        
        currentSum += nums[i];

        if(currentSum > maxSum) {
            maxSum = currentSum;
            maxStart = tempStart;
            maxEnd = i;
        }

        if(currentSum < 0) currentSum = 0;
    }
    
    return {maxStart, maxEnd};
}

```

---

# 6. From Brute Force to Optimized Solution

## Brute Force

### Idea

Use nested loops to calculate the sum of every possible contiguous subarray.

### Time Complexity

O(n²)

### Skeleton

```cpp
int maxSum = INT_MIN;
for(int i = 0; i < n; i++) {
    int currentSum = 0;
    for(int j = i; j < n; j++) {
        currentSum += arr[j];
        maxSum = max(maxSum, currentSum);
    }
}

```

---

## Optimized Approach

### Idea

Use a single loop. Add numbers, update the max, and reset if the running sum drops below zero.

### Time Complexity

O(n)

### Skeleton

```cpp
for(int i = 0; i < n; i++) {
    currentSum += arr[i];
    maxSum = max(maxSum, currentSum);
    if(currentSum < 0) currentSum = 0;
}

```

---

# 7. Complexity Analysis

| Complexity | Value | Reason |
| --- | --- | --- |
| **Time Complexity** | **O(n)** | We loop through the array exactly once. |
| **Space Complexity** | **O(1)** | We only use a few integer variables, regardless of how large the array is. |

---

# 8. Common Pitfalls

## Initializing `maxSum` to 0

If the array contains *only* negative numbers (e.g., `[-3, -5, -2]`), initializing `maxSum` to `0` will incorrectly return `0`. The correct answer is `-2`. Always initialize `maxSum` to `nums[0]` or `INT_MIN`.

---

## Resetting `currentSum` too early

You must update `maxSum` *before* checking if `currentSum < 0`. If a single negative number is the highest value in the array, you must capture it as your max before resetting your running total to zero.

---

# 9. Related Patterns

```text
Prefix Sum
     ↓
Sliding Window
     ↓
Kadane's Algorithm

```

While Sliding Window dynamically expands and shrinks from the left, Kadane's acts like a ruthless sliding window that completely collapses and starts over whenever the sum goes negative.

---

# 10. Pattern Recognition Flowchart

```text
Array?
        ↓
Need contiguous subset (subarray)?
        ↓
Need to Maximize the Sum?
        ↓
Array has negative numbers pulling the sum down?
        ↓
Think Kadane's Algorithm

```

---

# 11. Example Problem

## Problem Information

| Problem | Difficulty | Pattern |
| --- | --- | --- |
| [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) | Medium | Standard Kadane's |

---

### Intuition

We want the largest contiguous sum. If our current running total becomes negative, it acts as a burden to the next elements. We drop the burden by resetting the running total to 0 and starting fresh from the next element.

---

### Code

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSum = nums[0];
        int currentSum = 0;
        
        for(int i = 0; i < nums.size(); i++) {
            currentSum += nums[i];
            
            if(currentSum > maxSum) {
                maxSum = currentSum;
            }
            
            if(currentSum < 0) {
                currentSum = 0;
            }
        }
        
        return maxSum;
    }
};

```

---

### Dry Run

Input: `nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]`

| Step | Element | `currentSum` (before check) | `maxSum` | Is `currentSum` < 0? | Action on `currentSum` |
| --- | --- | --- | --- | --- | --- |
| 1 | **-2** | -2 | -2 | Yes | Reset to 0 |
| 2 | **1** | 1 | 1 | No | Keep as 1 |
| 3 | **-3** | 1 - 3 = -2 | 1 | Yes | Reset to 0 |
| 4 | **4** | 4 | 4 | No | Keep as 4 |
| 5 | **-1** | 4 - 1 = 3 | 4 | No | Keep as 3 |
| 6 | **2** | 3 + 2 = 5 | 5 | No | Keep as 5 |
| 7 | **1** | 5 + 1 = 6 | **6** | No | Keep as 6 |
| 8 | **-5** | 6 - 5 = 1 | 6 | No | Keep as 1 |
| 9 | **4** | 1 + 4 = 5 | 6 | No | Keep as 5 |

Output: `6` (The subarray is `[4, -1, 2, 1]`)

---

# How to Identify Kadane's (MOST IMPORTANT)

Use Kadane's Algorithm when you see:

* "Maximum subarray sum"
* "Largest contiguous sum"
* An array containing both positive and **negative** numbers.
* A requirement to maximize a contiguous score that can be reduced by penalties (negative values).

** Key Rule:** If you need the largest sum of a contiguous block, and negative numbers are pulling the running sum down → Kadane’s Algorithm.

---

#  Practice Problems

## Easy (Build Intuition)

| Problem | Link |
| --- | --- |
| Maximum Subarray | [https://leetcode.com/problems/maximum-subarray/](https://leetcode.com/problems/maximum-subarray/) |

---

## Intermediate (Strengthen Pattern Recognition)

| Problem | Link |
| --- | --- |
| Maximum Absolute Sum of Any Subarray | [https://leetcode.com/problems/maximum-absolute-sum-of-any-subarray/](https://leetcode.com/problems/maximum-absolute-sum-of-any-subarray/) |
| Maximum Sum Circular Subarray | [https://leetcode.com/problems/maximum-sum-circular-subarray/](https://leetcode.com/problems/maximum-sum-circular-subarray/) |

---

## Hard (Challenge Yourself)

| Problem | Link |
| --- | --- |
| Maximum Product Subarray | [https://leetcode.com/problems/maximum-product-subarray/](https://leetcode.com/problems/maximum-product-subarray/) |
