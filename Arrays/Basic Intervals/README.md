
# Basic Intervals: Overlaps and Merging

### Goal

Efficiently detect overlaps and combine touching ranges (intervals) within a 2D array by sorting them chronologically.

---

# 1. Core Idea

An interval is a range defined by two points: a `start` and an `end` (e.g., `[1, 5]`).

The core idea of Basic Intervals is **Sorting**. If you sort all intervals by their starting times, any intervals that overlap are guaranteed to end up right next to each other in the array.

Once sorted, you sweep through the array from left to right. If the current interval starts *before or exactly when* the previous interval ends, they overlap. You merge them into one single interval by extending the end time.

> Sort them by start time, then walk through and combine them like puddles of water.

---

# 2. Why This Pattern Matters

This pattern is the absolute foundation for all scheduling and timeline problems.

It helps:

1. Transform messy, overlapping timelines into a neat, disjointed set.
2. Form the prerequisite knowledge for much harder greedy scheduling and priority queue interval problems.
3. Handle interval data represented as a 2D array in a clean, logical sequence.

---

# 3. When to Use This Pattern

## The Checklist

Think about Basic Intervals when:

1. You are given a 2D array where each element is a pair `[start, end]`.
2. The problem talks about times, meetings, events, or physical coordinate ranges.
3. You need to find if items **overlap** or touch.
4. You need to **merge**, consolidate, or combine ranges.

---

## Key Phrases to Watch For

Problems containing phrases like:

* "Given an array of intervals..."
* "Merge all overlapping intervals..."
* "Return an array of the non-overlapping intervals..."
* "Determine if two events have conflict..."

---

# 4. Theory Behind the Pattern

Overlap detection follows a very strict mathematical rule.
Two intervals `A` and `B` (where `A` starts before `B`) will overlap if and only if:
`B.start <= A.end`

**Overlap condition to remember:**
`currentStart <= previousEnd`

By sorting the array based on the `start` value first, you guarantee that you only ever need to compare the current interval with the very last interval you processed.

---

# 5. Visualization & The Overlap Rule

Given: `[[1, 3], [8, 10], [2, 6]]`

1. **Sort by start time:**
`[[1, 3], [2, 6], [8, 10]]`
2. **Sweep and Merge:**
* Compare `[1, 3]` and `[2, 6]`. `2 <= 3`. They overlap!
* New merged interval: `[1, max(3, 6)]` -> `[1, 6]`.
* Compare `[1, 6]` and `[8, 10]`. `8 > 6`. No overlap!



---

# 6. Standard Template

This is the standard C++ boilerplate for sweeping and merging intervals.

```cpp
#include <vector>
#include <algorithm>
using namespace std;

vector<vector<int>> mergeIntervals(vector<vector<int>>& intervals) {
    if (intervals.empty()) return {};

    // 1. Sort intervals based on starting value
    sort(intervals.begin(), intervals.end());

    vector<vector<int>> merged;
    merged.push_back(intervals[0]); 

    for (int i = 1; i < intervals.size(); i++) {
        // merged.back() gets the last interval we successfully added
        if (intervals[i][0] <= merged.back()[1]) {
            // Overlap! Update the end time to be the maximum of both
            merged.back()[1] = max(merged.back()[1], intervals[i][1]);
        } else {
            // No overlap! Safely add the new distinct interval
            merged.push_back(intervals[i]);
        }
    }
    
    return merged;
}

```

---

# 7. Complexity Analysis

| Complexity | Value | Reason |
| --- | --- | --- |
| **Time Complexity** | **O(n log n)** | Sorting the array takes O(n log n). The linear sweep afterward is just O(n). |
| **Space Complexity** | **O(n)** | In the worst case (no overlaps exist), the `merged` array will contain all `n` intervals. |

---

# 8. Common Pitfalls

## Forgetting to Sort

If you do not sort the intervals by their start time first, checking `intervals[i][0] <= merged.back()[1]` completely fails because chronological order is not guaranteed. Always sort first.

---

## Not using `max()` for the End Time

When intervals overlap, do not just assume the second interval has a larger end time!
Example: `[1, 10]` and `[2, 3]`. If you just take the second interval's end time, you get `[1, 3]`, which is wrong. You must use `max(10, 3)` to get the correct merged interval `[1, 10]`.

---

# 9. Related Patterns

```text
Sorting
     ↓
Intervals
     ↓
Greedy Interval Problems

```

---

# 10. Pattern Recognition Flowchart

```text
2D Array?
        ↓
Do the elements represent [start, end] ranges?
        ↓
Need to combine them or check if they touch?
        ↓
Think Basic Intervals (Sort by Start -> Sweep and Merge)

```

---

# 11. Example Problem

## Problem Information

| Problem | Difficulty | Pattern |
| --- | --- | --- |
| [Merge Intervals](https://leetcode.com/problems/merge-intervals/) | Medium | Basic Merge |

---

### Intuition

If we line all the events up in chronological order (sort by start time), we can just walk down the timeline. If the next event starts before our current event finishes, they belong together in one big event.

---

### Code (C++)

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        if(intervals.size() <= 1) return intervals;
        
        sort(intervals.begin(), intervals.end());
        
        vector<vector<int>> result;
        result.push_back(intervals[0]);
        
        for(int i = 1; i < intervals.size(); i++) {
            if(intervals[i][0] <= result.back()[1]) {
                result.back()[1] = max(result.back()[1], intervals[i][1]);
            } else {
                result.push_back(intervals[i]);
            }
        }
        
        return result;
    }
};

```

---

### Dry Run

Input: `intervals = [[1,3], [8,10], [2,6], [15,18]]`

| Step | Action | Sorted Array State | `result` Array State |
| --- | --- | --- | --- |
| 1 | Sort the intervals | `[[1,3], [2,6], [8,10], [15,18]]` | Empty |
| 2 | Push first element | `[[1,3], ...]` | `[[1,3]]` |
| 3 | Read `[2,6]`. `2 <= 3` (Overlap!) | Update end to `max(3, 6)` | `[[1,6]]` |
| 4 | Read `[8,10]`. `8 > 6` (No Overlap) | Push to result | `[[1,6], [8,10]]` |
| 5 | Read `[15,18]`. `15 > 10` (No Overlap) | Push to result | `[[1,6], [8,10], [15,18]]` |

Output: `[[1,6], [8,10], [15,18]]`

---

#  How to Identify Basic Intervals (MOST IMPORTANT)

Use Basic Intervals when you see:

* A 2D array formatted as pairs `[start, end]`.
* Problems dealing with times, meetings, schedules, or coordinate ranges.
* The words "Overlap", "Merge", or "Combine".

** Key Rule:** If you are dealing with ranges of numbers and need to find where they cross over each other to form larger groups → Sort by **Start Time** and Merge.

---

# 12. Practice Problems

This progression moves naturally from basic overlap detection to the threshold of greedy algorithm interval problems.

| Order | Problem | Difficulty | Link |
| --- | --- | --- | --- |
| 1 | Determine if Two Events Have Conflict | Easy | [https://leetcode.com/problems/determine-if-two-events-have-conflict/](https://leetcode.com/problems/determine-if-two-events-have-conflict/) |
| 2 | Summary Ranges | Easy | [https://leetcode.com/problems/summary-ranges/](https://leetcode.com/problems/summary-ranges/) |
| 3 | Merge Intervals | Medium | [https://leetcode.com/problems/merge-intervals/](https://leetcode.com/problems/merge-intervals/) |
| 4 | Insert Interval | Medium | [https://leetcode.com/problems/insert-interval/](https://leetcode.com/problems/insert-interval/) |
| 5 | Non-overlapping Intervals | Medium | [https://leetcode.com/problems/non-overlapping-intervals/](https://leetcode.com/problems/non-overlapping-intervals/) |
| 6 | Minimum Number of Arrows to Burst Balloons | Medium | [https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/) |
