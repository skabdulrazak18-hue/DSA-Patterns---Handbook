# Hashing: Fast Lookups and Frequency Counting

### Goal

Store data in a way that allows you to instantly check if an element exists, count its frequency, or look up its associated value without scanning the entire array.

---

# 1. Core Idea

The Hashing pattern is a space-for-time tradeoff. Instead of using a nested loop to search for an item (which takes O(n) time), you store items in a **Hash Set** or **Hash Map** as you iterate through the array.

When you need to find an item later, you can look it up instantly.

> Store what you have seen so far, so you can instantly answer questions about the past without looking back.

---

# 2. Why This Pattern Matters

Hashing is the ultimate tool for optimizing search and lookup operations.

It helps:

1. Transform O(n²) brute-force search solutions into O(n) linear solutions.
2. Reduce lookup times from O(n) to O(1).
3. Easily group or organize data based on common properties (like grouping anagrams).
4. Track frequencies and counts efficiently.

---

# 3. When to Use This Pattern

## The Checklist

Think about Hashing when:

✓ You need to look up previous elements instantly.
✓ You need to check for the existence of an element.
✓ You need to count the frequency of elements or characters.
✓ You are asked to find duplicates or unique elements.
✓ You need to find a specific pair of elements (like summing to a target).

---

## Key Phrases to Watch For

Problems containing phrases like:

* "Check if an element exists..."
* "Find a pair of elements..."
* "Count the frequency..."
* "Find the first unique character..."
* "Contains duplicates..."
* "Group by..."

---

# 4. Theory Behind the Pattern

Under the hood, a Hash Map or Hash Set uses a "hash function." This function takes your data (the key) and mathematically converts it into a direct memory address (an index).

Because it calculates the exact location mathematically, it doesn't need to scan through all the other elements to find what you are looking for. It jumps straight to the answer in constant O(1) time.

---

# 5. Common Variations

---

## A. Hash Set (For Uniqueness / Existence)

Use an `unordered_set` when you only care *if* an element exists, or if you need to filter out duplicates.

### Visualization

Filter out duplicates from `[1, 2, 2, 3]`

```text
Add 1 -> Set: {1}
Add 2 -> Set: {1, 2}
Add 2 -> Set already has 2! (Duplicate found)

```

### Template

```cpp
#include <unordered_set>

bool containsDuplicate(vector<int>& nums) {
    unordered_set<int> seen;

    for(int num : nums) {
        // If we already saw the number, it's a duplicate
        if(seen.count(num)) {
            return true;
        }
        seen.insert(num);
    }
    
    return false;
}

```

---

## B. Hash Map (For Frequencies / Key-Value Mapping)

Use an `unordered_map` when you need to count how many times something appears, or link a key to a specific value (like linking a number to its original index).

### Visualization

Count frequencies in `[a, b, a, c]`

```text
Read 'a' -> Map: {a: 1}
Read 'b' -> Map: {a: 1, b: 1}
Read 'a' -> Map: {a: 2, b: 1}
Read 'c' -> Map: {a: 2, b: 1, c: 1}

```

### Template

```cpp
#include <unordered_map>

int findMostFrequent(vector<int>& nums) {
    unordered_map<int, int> freq;

    for(int num : nums) {
        freq[num]++; // Increment the count for this number
    }

    int maxCount = 0;
    int mostFrequentNum = -1;

    for(auto pair : freq) {
        if(pair.second > maxCount) {
            maxCount = pair.second;
            mostFrequentNum = pair.first;
        }
    }
    
    return mostFrequentNum;
}

```

---

# 6. From Brute Force to Optimized Solution

## Brute Force

### Idea

Use nested loops to check every possible pair.

### Time Complexity

O(n²)

### Skeleton

```cpp
for(int i = 0; i < n; i++) {
    for(int j = i + 1; j < n; j++) {
        if(arr[i] + arr[j] == target) {
            // Found pair
        }
    }
}

```

---

## Optimized Approach

### Idea

Use a Hash Map to store what we have seen. For each element, instantly check if its needed counterpart is already in the map.

### Time Complexity

O(n)

### Skeleton

```cpp
unordered_map<int, int> seen;

for(int i = 0; i < n; i++) {
    int needed = target - arr[i];
    if(seen.count(needed)) {
        // Found pair
    }
    seen[arr[i]] = i;
}

```

---

# 7. Complexity Analysis

| Complexity | Value | Reason |
| --- | --- | --- |
| **Time Complexity** | **O(n)** | Iterating through the array takes O(n). Inserting/looking up elements in an `unordered_map` or `unordered_set` takes O(1) on average. |
| **Space Complexity** | **O(n)** | In the worst-case scenario, you might need to store every single unique element from the array into your map or set. |

---

# 8. Common Pitfalls

## Using `map` instead of `unordered_map`

In C++, `std::map` is implemented as a balanced tree and takes O(log n) time per lookup. `std::unordered_map` uses actual hashing and takes O(1) time. Always use `unordered_map` unless you specifically need the keys to be sorted!

---

## Memory Limits

Because Hashing trades space for time, storing massive amounts of data can occasionally cause a Memory Limit Exceeded (MLE) error in very strictly constrained environments.

---

# 9. Related Patterns

```text
Two Pointers
     ↓
Hashing
     ↓
Sliding Window with Hashing

```

Hashing is often combined with Sliding Window to track the frequency of characters inside the current window boundary.

---

# 10. Pattern Recognition Flowchart

```text
Array or String?
        ↓
Need O(1) lookups instead of searching?
        ↓
Need frequencies, pairs, or uniqueness?
        ↓
Think Hashing

```

---

# 11. Example Problem

## Problem Information

| Problem | Difficulty | Pattern |
| --- | --- | --- |
| [Two Sum](https://leetcode.com/problems/two-sum/) | Easy | Hash Map (Key-Value Mapping) |

---

### Intuition

Instead of checking `4+2`, `4+7`, `4+1`, etc. using nested loops, we store numbers and their indices as we see them. For every new number, we calculate exactly what number we *need* to hit the target, and we ask the Hash Map if we have seen it yet.

---

### Code

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        // Maps the number value to its index in the array
        unordered_map<int, int> seen; 
        
        for(int i = 0; i < nums.size(); i++) {
            int needed = target - nums[i];
            
            // If the needed number is in our map, we found the pair
            if(seen.count(needed)) {
                return {seen[needed], i};
            }
            
            // Otherwise, store the current number and its index
            seen[nums[i]] = i;
        }
        
        return {};
    }
};

```

---

### Dry Run

Input: `nums = [2, 7, 11, 15]`, `target = 9`

| Step | Current Value `nums[i]` | Needed `(9 - value)` | Is Needed in Map? | Action | Map State |
| --- | --- | --- | --- | --- | --- |
| 1 | 2 | 7 | No | Store 2 at index 0 | `{2: 0}` |
| 2 | 7 | 2 | **Yes! (Index 0)** | Return `{0, 1}` | `{2: 0}` |

Output: `[0, 1]`

---

#  How to Identify Hashing (MOST IMPORTANT)

Use Hashing when you see:

* "Check if an element exists"
* "Find a pair of elements" (like Two Sum)
* "Count the frequency of elements or characters"
* "Find duplicates or unique elements"
* Problems asking for intersections or mappings between two sets of data

**👉 Key Rule:** If you need to look up previous elements instantly, check for existence, or count occurrences → Hashing.

---

# 12. Practice Problems

## Easy (Build Intuition)

| Problem | Link |
| --- | --- |
| Two Sum | [https://leetcode.com/problems/two-sum/](https://leetcode.com/problems/two-sum/) |
| Contains Duplicate | [https://leetcode.com/problems/contains-duplicate/](https://leetcode.com/problems/contains-duplicate/) |
| Valid Anagram | [https://leetcode.com/problems/valid-anagram/](https://leetcode.com/problems/valid-anagram/) |

---

## Intermediate (Strengthen Pattern Recognition)

| Problem | Link |
| --- | --- |
| Group Anagrams | [https://leetcode.com/problems/group-anagrams/](https://leetcode.com/problems/group-anagrams/) |
| Top K Frequent Elements | [https://leetcode.com/problems/top-k-frequent-elements/](https://leetcode.com/problems/top-k-frequent-elements/) |
| Longest Consecutive Sequence | [https://leetcode.com/problems/longest-consecutive-sequence/](https://leetcode.com/problems/longest-consecutive-sequence/) |

---

## Hard (Challenge Yourself)

| Problem | Link |
| --- | --- |
| Substring with Concatenation of All Words | [https://leetcode.com/problems/substring-with-concatenation-of-all-words/](https://leetcode.com/problems/substring-with-concatenation-of-all-words/) |
| Max Points on a Line | [https://leetcode.com/problems/max-points-on-a-line/](https://leetcode.com/problems/max-points-on-a-line/) |
