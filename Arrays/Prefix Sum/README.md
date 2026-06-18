

# Prefix Sum: Precomputing to Answer Queries Instantly

### Goal

Precalculate cumulative sums of an array into a new array, allowing you to find the sum of any contiguous subarray in O(1) time instead of recalculating it.

---

# 1. Core Idea

The Prefix Sum pattern involves creating a new array where the value at index `i` is the sum of all elements from the beginning of the original array up to index `i`.

Once this precomputation is done, the sum of any subarray from index `L` to index `R` can be found instantly using a simple subtraction:

> `Sum[L to R] = Prefix[R] - Prefix[L - 1]`

---

# 2. Why This Pattern Matters

Prefix Sum is an essential optimization technique for Array problems.

It helps:

1. Turn O(n) range sum calculations into O(1) instant lookups.
2. Optimize algorithms that require multiple queries on the same array.
3. Solve problems involving subarrays with negative numbers (where Sliding Window fails).
4. Build the foundation for complex 2D matrix calculations.

---

# 3. When to Use This Pattern

## The Checklist

Think about Prefix Sum when:
1. The problem involves arrays.
2. You need to calculate the sum, product, or XOR of contiguous subarrays.
3. You are asked to answer *multiple* queries about subarray sums.
4. The array contains negative numbers, and you need to find a target sum (Sliding Window only works well with positive numbers).

---

## Key Phrases to Watch For

Problems containing phrases like:

* "Sum of elements between index i and j..."
* "Subarray sum equals k..."
* "Multiple range queries..."
* "Number of subarrays with an odd sum..."

---

# 4. Theory Behind the Pattern

If you want to find the sum of elements from index 2 to index 5, you don't need to add `arr[2] + arr[3] + arr[4] + arr[5]`.

Instead, if you know:

1. The total sum from index 0 to 5.
2. The total sum from index 0 to 1.

You can simply subtract the sum of (0 to 1) from the sum of (0 to 5) to isolate the exact chunk you want (2 to 5). This subtraction takes constant time, dramatically speeding up algorithms.

---

# 5. Common Variations

---

## A. Standard 1D Prefix Sum

Used for answering multiple range queries efficiently.

### Visualization

Original Array:

```text
[2,  4,  1,  5,  3]

```

Prefix Array (Running Total):

```text
[2,  6,  7, 12, 15]

```

To find the sum from index 1 to 3:
`Prefix[3] - Prefix[0]` = `12 - 2 = 10`

### Template (C++)

```cpp
vector<int> buildPrefix(vector<int>& arr) {
    int n = arr.size();
    vector<int> prefix(n);
    prefix[0] = arr[0];
    
    for(int i = 1; i < n; i++) {
        prefix[i] = prefix[i - 1] + arr[i];
    }
    return prefix;
}

// Query function
int rangeSum(vector<int>& prefix, int L, int R) {
    if (L == 0) return prefix[R];
    return prefix[R] - prefix[L - 1];
}

```

---

## B. Prefix Sum with Hash Map

Used when you need to find *how many* subarrays sum to a specific target `k`, especially when negative numbers are involved.

### How It Works

As we calculate the running prefix sum, we store how many times we have seen each prefix sum in a Hash Map. If `current_sum - k` exists in our Hash Map, it means we have found a valid subarray!

### Template (C++)

```cpp
int subarraySum(vector<int>& nums, int k) {
    unordered_map<int, int> prefixCounts;
    prefixCounts[0] = 1; // Base case: one way to sum to 0
    
    int currentSum = 0;
    int count = 0;
    
    for(int num : nums) {
        currentSum += num;
        
        if (prefixCounts.find(currentSum - k) != prefixCounts.end()) {
            count += prefixCounts[currentSum - k];
        }
        
        prefixCounts[currentSum]++;
    }
    
    return count;
}

```

---

# 6. From Brute Force to Optimized Solution

## Brute Force

### Idea

For every query, loop through the array from `L` to `R` and add up the elements.

### Time Complexity

O(q * n) where `q` is the number of queries.

### Skeleton

```cpp
for(auto query : queries) {
    int L = query[0], R = query[1];
    int sum = 0;
    for(int i = L; i <= R; i++) {
        sum += arr[i];
    }
}

```

---

## Optimized Approach

### Idea

Build a Prefix array once. Then answer every query instantly using subtraction.

### Time Complexity

O(n + q) — O(n) to build, O(1) for each of the `q` queries.

### Skeleton

```cpp
// 1. Build Prefix array once
// 2. For each query: answer = prefix[R] - prefix[L-1]

```

---

# 7. Complexity Analysis

| Complexity | Value | Reason |
| --- | --- | --- |
| **Time Complexity** | **O(n)** | It takes one pass through the array to build the prefix sums. Querying is O(1). |
| **Space Complexity** | **O(n)** | We usually need to create a new array or a Hash Map of size `n` to store the prefix data. |

---

# 8. Common Pitfalls

## The L-1 Bounds Issue

When querying the range `[L, R]`, the formula is `Prefix[R] - Prefix[L-1]`. If `L` is `0`, `L-1` becomes `-1`, causing an Array Out Of Bounds error. Always check `if (L == 0)` or use a 1-indexed prefix array to prevent this.

## Integer Overflow

Prefix sums grow very large, very fast. If the original array contains large positive numbers, the cumulative sum might exceed the maximum limit of a standard 32-bit `int`. Always consider using `long long` for the prefix array to prevent overflow.

---

# 9. Related Patterns

```text
Prefix Sum
     ↓
Prefix Sum + Hash Map (For target sum counts)
     ↓
2D Prefix Sum (For matrix range queries)

```

Prefix Sum solves the negative number flaw of Sliding Window. If an array has negative numbers and asks for a target subarray sum, abandon Sliding Window and use Prefix Sum + Hash Map.

---

# 10. Pattern Recognition Flowchart

```text
Array problem?
        ↓
Need the sum/product of contiguous elements?
        ↓
Are there multiple queries OR negative numbers?
        ↓
Think Prefix Sum

```

---

# 11. Example Problem

## Problem Information

| Problem | Difficulty | Pattern |
| --- | --- | --- |
| [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/) | Easy | Standard 1D Prefix Sum |

---

### Intuition

We will be asked for the sum between index `left` and `right` multiple times. Recalculating the sum every time is too slow. We will precompute the running total of the array in the constructor so that every `sumRange` call takes O(1) time.

---

### Code (C++)

```cpp
class NumArray {
private:
    vector<int> prefix;
    
public:
    NumArray(vector<int>& nums) {
        int n = nums.size();
        prefix.resize(n);
        
        prefix[0] = nums[0];
        for(int i = 1; i < n; i++) {
            prefix[i] = prefix[i - 1] + nums[i];
        }
    }
    
    int sumRange(int left, int right) {
        if (left == 0) return prefix[right];
        
        return prefix[right] - prefix[left - 1];
    }
};

```

---

### Dry Run

Input Array: `nums = [-2, 0, 3, -5, 2, -1]`

**Constructor Phase:**

| Index | Current Value | Running Total (Prefix Array) |
| --- | --- | --- |
| 0 | -2 | `[-2]` |
| 1 | 0 | `[-2, -2]` |
| 2 | 3 | `[-2, -2, 1]` |
| 3 | -5 | `[-2, -2, 1, -4]` |
| 4 | 2 | `[-2, -2, 1, -4, -2]` |
| 5 | -1 | `[-2, -2, 1, -4, -2, -3]` |

**Query Phase: `sumRange(2, 5)**`

* `left = 2`, `right = 5`
* Formula: `prefix[5] - prefix[1]`
* Calculation: `-3 - (-2)` = `-1`

---

# 12. Practice Problems

## Easy (Build Intuition)

| Problem | Link |
| --- | --- |
| Range Sum Query - Immutable | [https://leetcode.com/problems/range-sum-query-immutable/](https://leetcode.com/problems/range-sum-query-immutable/) |
| Find Pivot Index | [https://leetcode.com/problems/find-pivot-index/](https://leetcode.com/problems/find-pivot-index/) |
| Running Sum of 1d Array | [https://leetcode.com/problems/running-sum-of-1d-array/](https://leetcode.com/problems/running-sum-of-1d-array/) |

## Intermediate (Strengthen Pattern Recognition)

| Problem | Link |
| --- | --- |
| Subarray Sum Equals K | [https://leetcode.com/problems/subarray-sum-equals-k/](https://leetcode.com/problems/subarray-sum-equals-k/) |
| Product of Array Except Self | [https://leetcode.com/problems/product-of-array-except-self/](https://leetcode.com/problems/product-of-array-except-self/) |
| Subarray Sums Divisible by K | [https://leetcode.com/problems/subarray-sums-divisible-by-k/](https://leetcode.com/problems/subarray-sums-divisible-by-k/) |

## Hard (Challenge Yourself)

| Problem | Link |
| --- | --- |
| Max Sum of Rectangle No Larger Than K | [https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k/](https://leetcode.com/problems/max-sum-of-rectangle-no-larger-than-k/) |
| Number of Submatrices That Sum to Target | [https://leetcode.com/problems/number-of-submatrices-that-sum-to-target/](https://leetcode.com/problems/number-of-submatrices-that-sum-to-target/) |
