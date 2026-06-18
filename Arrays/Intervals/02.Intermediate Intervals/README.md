

---

# Intermediate Intervals: Greedy Scheduling

### Goal

Make optimal decisions on an interval-based timeline (e.g., maximizing the number of events attended or minimizing resource removals) using greedy strategies.

---

# 1. Core Idea

Intermediate Interval problems move beyond simple merging; they require **selection**. We must choose which intervals to keep or remove to satisfy a specific objective.

The secret is the **Greedy Choice**:

* To **maximize non-overlapping events**, always pick the interval that **ends earliest**. By finishing as early as possible, you minimize the "footprint" left on the timeline, leaving the maximum amount of space for future events.

> Sort by end time, then greedily pick the earliest finisher to maximize your options.

---

# 2. Why This Pattern Matters

This pattern teaches you how to move from "organizing data" to "optimizing outcomes."

It helps:

1. Solve scheduling conflicts in $O(n \log n)$ time.
2. Build intuition for Greedy Algorithms, which are crucial for complex system design.
3. Handle interval data represented as a 2D array in an optimized, logical sequence.

---

# 3. When to Use This Pattern

## The Checklist

Think about Intermediate Intervals when:
1. You are given a 2D array of `[start, end]` intervals.
2. You are asked to **maximize** the number of meetings or events.
3. You are asked to **minimize** the number of removals to resolve overlaps.
4. You are asked to find the minimum resources (arrows/lines) to pierce all intervals.

## Key Phrases to Watch For

* "Maximum number of non-overlapping intervals/meetings..."
* "Minimum number of intervals to remove..."
* "Find the minimum number of arrows to burst all balloons..."
* "Can a person attend all meetings?"

---

# 4. Theory Behind the Pattern

The **Greedy Choice Property** is the theoretical backbone. If you have two events, `A [1, 10]` and `B [2, 3]`:

* If you pick `A`, you block the timeline until `10`.
* If you pick `B`, you finish by `3`, leaving the timeline from `3` to `infinity` wide open.

By always finishing as early as possible, you maximize your chances of fitting in the next event.

---

# 5. Greedy Decision Logic

In intermediate problems, we must make a decision based on the `lastEndTime`:

* **Conflict Condition:** `currentStart < lastEndTime`
* **If Conflict:** * Keep the interval that finishes earlier.
* Remove the interval that blocks more future choices.


* **If No Conflict:** Attend the interval and update `lastEndTime` to the current interval's end.

---

# 6. Greedy Selection Template

*Used for: Non-overlapping Intervals, Burst Balloons*

```cpp
sort(intervals.begin(), intervals.end(), [](auto& a, auto& b) {
    return a[1] < b[1]; // Sort by end time
});
int count = 0;
int lastEnd = intervals[0][1];
for (int i = 1; i < intervals.size(); i++) {
    if (intervals[i][0] < lastEnd) {
        count++; // Conflict: remove or fire arrow
    } else {
        lastEnd = intervals[i][1]; // Update boundary
    }
}

```

---

# 7. Meeting Rooms I Template

*Used for: Meeting Rooms I*

```cpp
sort(intervals.begin(), intervals.end()); // Sort by start time
for (int i = 1; i < intervals.size(); i++) {
    if (intervals[i][0] < intervals[i-1][1]) {
        return false; // Collision detected
    }
}
return true;

```

---

# 8. Complexity Analysis

| Complexity | Value | Reason |
| --- | --- | --- |
| **Time Complexity** | **$O(n \log n)$** | Sorting dominates the complexity. The greedy sweep is $O(n)$. |
| **Space Complexity** | **$O(1)$** | Constant space required for the selection logic (no extra arrays needed). |

---

# 9. Common Pitfalls

* **Sorting by Start Time:** Only use start time sorting for *merging* (Basic). For *selection/removal* (Intermediate), you must sort by **end time**.
* **Boundary conditions:** Ensure you handle equality correctly (`<` vs `<=`). If one meeting ends exactly when another starts, it is typically NOT an overlap.

---

# 10. Related Patterns

```text
Sorting
     ↓
Intervals
     ↓
Greedy Algorithm

```

---

# 11. Pattern Recognition Flowchart

```text
2D Array of [start, end]?
        ↓
Need to MERGE ranges? -> Basic Intervals
        ↓
Otherwise:
    • Need to maximize meetings?
    • Need to minimize removals?
    • Need minimum arrows/resources?
        ↓
Sort by END Time -> Greedy Sweep

```

---


---

# 12. Example Problem: Non-overlapping Intervals

**Intuition:** To keep the maximum number of intervals, remove the minimum number of overlapping ones. By sorting by end time, we ensure we always leave as much room as possible for the next event.

**Code (C++):**

```cpp
int eraseOverlapIntervals(vector<vector<int>>& intervals) {
    sort(intervals.begin(), intervals.end(), [](auto& a, auto& b) {
        return a[1] < b[1];
    });
    int count = 0, lastEnd = intervals[0][1];
    for (int i = 1; i < intervals.size(); i++) {
        if (intervals[i][0] < lastEnd) count++; // Conflict!
        else lastEnd = intervals[i][1];
    }
    return count;
}

```

**Dry Run:** `intervals = [[1,2], [2,3], [3,4], [1,3]]`

| Step | Action | `lastEnd` | `count` (Removed) | Note |
| --- | --- | --- | --- | --- |
| 1 | Sort by End | - | 0 | Sorted: `[[1,2], [2,3], [1,3], [3,4]]` |
| 2 | Attend `[1,2]` | 2 | 0 | First interval kept |
| 3 | Check `[2,3]` | 3 | 0 | `2 >= 2` (No conflict) |
| 4 | Check `[1,3]` | 3 | 1 | `1 < 3` (Conflict!) |
| 5 | Check `[3,4]` | 4 | 1 | `3 >= 3` (No conflict) |


Complexity Analysis
Complexity	Value	Reason
Time Complexity	O(n log n)	Sorting the intervals takes O(n log n), and scanning them once takes O(n). Sorting dominates the total complexity.
Space Complexity	O(1)	We only use a few variables to keep track of the answer and do not create any extra data structures.
--------
# . Identifying Intermediate Problems

* You are asked to count how many items to "remove."
* You are asked to fit the "maximum number of meetings" possible.
* You need to "find the minimum number of points" to pierce all intervals (e.g., "minimum number of arrows").
----------
**Practice Problems**

| Order | Problem | Difficulty | Link |
| --- | --- | --- | --- |
| 1 | Non-overlapping Intervals | Medium | [Link](https://leetcode.com/problems/non-overlapping-intervals/) |
| 2 | Meeting Rooms I | Medium | (LeetCode Premium) |
| 3 | Min Arrows to Burst Balloons | Medium | [Link](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/) |

---

