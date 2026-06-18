

# Basic Intervals: Overlaps and Merging

### Goal

Efficiently detect overlaps and combine touching ranges (intervals) within a 2D array by sorting them chronologically.

---

# 1. Core Idea

An interval is a range defined by two points: a `start` and an `end` (e.g., `[1, 5]`).

The core idea of Basic Intervals is **Sorting**. If you sort all intervals by their starting times, any intervals that overlap are guaranteed to end up right next to each other in the array.

> Sort by start time, then walk through and combine them like puddles of water.

---

# 2. Why This Pattern Matters

This pattern is the absolute foundation for all scheduling and timeline problems.

It helps:

1. Transform messy, overlapping timelines into a neat, disjointed set.
2. Form the prerequisite knowledge for much harder greedy scheduling problems.
3. Handle interval data represented as a 2D array in a clean, logical sequence.

---

# 3. When to Use This Pattern

## The Checklist

Think about Basic Intervals when:

1. You are given a 2D array of `[start, end]` intervals.
2. You are asked to detect if any items **overlap** or touch.
3. You need to **merge** or combine ranges into disjoint sets.
4. You need to insert a new range into an existing sorted timeline.

---

## Key Phrases to Watch For

* "Merge all overlapping intervals..."
* "Return an array of the non-overlapping intervals..."
* "Determine if two events have conflict..."
* "Insert a new interval into the existing sorted intervals..."

---

# 4. Theory Behind the Pattern

Overlap detection follows a strict mathematical rule. Two intervals `A` and `B` (where `A` starts before `B`) overlap if and only if: `B.start <= A.end`.

**Overlap condition to remember:** `currentStart <= previousEnd`

By sorting based on the `start` value, you guarantee that you only need to compare the current interval with the last interval you processed.

---

# 5. Merge Logic

Given: `[[1, 3], [8, 10], [2, 6]]`

1. **Sort by start time:** `[[1, 3], [2, 6], [8, 10]]`
2. **Sweep and Merge:**
* Compare `[1, 3]` and `[2, 6]`. `2 <= 3`. They overlap!
* New merged interval: `[1, max(3, 6)]` -> `[1, 6]`.
* Compare `[1, 6]` and `[8, 10]`. `8 > 6`. No overlap!



---

# 6. Standard Merge Template

```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end()); // 1. Sort by Start
    vector<vector<int>> res;
    res.push_back(intervals[0]);
    for(int i = 1; i < intervals.size(); i++) {
        if(intervals[i][0] <= res.back()[1]) { // 2. Overlap Check
            res.back()[1] = max(res.back()[1], intervals[i][1]); // 3. Merge
        } else {
            res.push_back(intervals[i]);
        }
    }
    return res;
}

```

---

# 7. Insert Interval Template

Use this when the array is already sorted to keep the complexity at **O(n)**.

```cpp
// Phase 1: Push intervals ending before newInterval starts
// Phase 2: Merge overlapping intervals
while(i < n && intervals[i][0] <= newInterval[1]) {
    newInterval[0] = min(newInterval[0], intervals[i][0]);
    newInterval[1] = max(newInterval[1], intervals[i][1]);
    i++;
}

```

---

# 8. Complexity Analysis

| Complexity | Value | Reason |
| --- | --- | --- |
| **Time Complexity** | **O(n log n)** | Sorting takes O(n log n). The sweep/scan is O(n). |
| **Space Complexity** | **O(n)** | In the worst case, the result array stores all intervals. |

---

# 9. Common Pitfalls

* **Forgetting to Sort:** If you do not sort by start time, the sweep logic fails.
* **Not using `max()` for End Time:** When merging, ensure you capture the furthest possible end time using `max(lastEnd, currentEnd)`.
* **Using `<` instead of `<=`:** Intervals `[1,3]` and `[3,5]` overlap because `currentStart <= previousEnd`. Boundary mistakes are common.

---

# 10. Related Patterns

```text
Sorting
     ↓
Intervals
     ↓
Greedy Interval Problems

```

---

# 11. Pattern Recognition Flowchart

```text
2D Array of [start, end]?
        ↓
Need to merge, combine, or insert ranges?
        ↓
Sort by Start Time
        ↓
Sweep and Merge

```

---

# 12.1 Example Problem: Merge Intervals

### Intuition

If we line all the events up in chronological order, we can just walk down the timeline. If the next event starts before our current event finishes, they belong together in one big event.

### Code (C++)

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        if(intervals.size() <= 1) return intervals;
        sort(intervals.begin(), intervals.end()); // 1. Sort
        vector<vector<int>> res;
        res.push_back(intervals[0]);
        for(int i = 1; i < intervals.size(); i++) {
            if(intervals[i][0] <= res.back()[1]) { // 2. Overlap
                res.back()[1] = max(res.back()[1], intervals[i][1]);
            } else {
                res.push_back(intervals[i]);
            }
        }
        return res;
    }
};

```

### Dry Run Table

Input: `[[1,3], [8,10], [2,6], [15,18]]`

| Step | Action | Sorted Array State | `result` State |
| --- | --- | --- | --- |
| 1 | Sort | `[[1,3], [2,6], [8,10], [15,18]]` | Empty |
| 2 | Push 1st | `[[1,3], ...]` | `[[1,3]]` |
| 3 | Compare `[2,6]` | `2 <= 3` (Overlap!) | `[[1,6]]` |
| 4 | Compare `[8,10]` | `8 > 6` (No Overlap) | `[[1,6], [8,10]]` |
| 5 | Compare `[15,18]` | `15 > 10` (No Overlap) | `[[1,6], [8,10], [15,18]]` |

### Complexity Analysis

Complexity Analysis
Time Complexity: O(n log n), where n is the number of intervals.

The sort() function dominates the complexity by taking O(n log n) time.

The subsequent for loop performs a linear pass through the sorted array, taking O(n) time.

Total time = O(n log n) + O(n) = O(n log n).

Space Complexity: O(n).

In the worst case (where no intervals overlap), the result vector will store all n intervals from the input, requiring O(n) extra space.

---
Identifying Basic Interval Problems
Problems with a 2D array formatted as [start, end].

Problems involving "merging," "combining," or "inserting" ranges.

Problems where you must check if any two events overlap or conflict.
--------------
# 12.2. Practice Problems

### Easy (Build Intuition)

| Order | Problem | Link |
| --- | --- | --- |
| 1 | Determine if Two Events Have Conflict | [https://leetcode.com/problems/determine-if-two-events-have-conflict/](https://leetcode.com/problems/determine-if-two-events-have-conflict/) |
| 2 | Summary Ranges | [https://leetcode.com/problems/summary-ranges/](https://leetcode.com/problems/summary-ranges/) |

### Medium (Mastering Patterns)

| Order | Problem | Link |
| --- | --- | --- |
| 3 | Merge Intervals | [https://leetcode.com/problems/merge-intervals/](https://leetcode.com/problems/merge-intervals/) |
| 4 | Insert Interval | [https://leetcode.com/problems/insert-interval/](https://leetcode.com/problems/insert-interval/) |

